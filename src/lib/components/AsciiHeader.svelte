<script lang="ts">
    import { onMount } from "svelte";

    // --- CONFIGURATION ---
    const DENSITY = " .:-=+*#%@";

    // --- STATE (RUNES) ---
    let asciiText = $state("");
    let width = $state(0);
    let height = $state(0);

    // Element References
    let canvasRef: HTMLCanvasElement;
    let cardRef: HTMLDivElement;

    // Animation State
    let time = 0;
    let animationId: number;

    interface Star {
        x: number;
        y: number;
        size: number;
    }

    interface ShootingStar {
        x: number;
        y: number;
        vx: number;
        vy: number;
        life: number;
    }

    const stars: Star[] = [];
    const shootingStars: ShootingStar[] = [];

    // --- THE ENGINE ---

    function initStars() {
        stars.length = 0;
        // Balanced density for the header card
        const starCount = Math.floor(width * height * 0.008);
        for (let i = 0; i < starCount; i++) {
            stars.push({
                x: Math.random() * width,
                y: Math.random() * height * 0.8, // Stars mostly everywhere
                size: Math.random() > 0.95 ? 2 : 1,
            });
        }
    }

    function spawnShootingStar() {
        if (Math.random() > 0.98) {
            shootingStars.push({
                x: Math.random() * width,
                y: Math.random() * height * 0.4,
                vx: 0.2 + Math.random() * 0.4,
                vy: 0.1 + Math.random() * 0.2,
                life: 1.0,
            });
        }
    }

    function drawScene(ctx: CanvasRenderingContext2D) {
        if (width <= 0 || height <= 0) return;

        // 1. Clear Background
        ctx.fillStyle = "#000000";
        ctx.fillRect(0, 0, width, height);

        // 2. Stars
        ctx.fillStyle = "#ffffff";
        stars.forEach((star) => {
            const opacity = 0.5 + Math.sin(time * 2 + star.x) * 0.5;
            ctx.globalAlpha = opacity;
            ctx.fillRect(star.x, star.y, star.size, star.size);
        });
        ctx.globalAlpha = 1.0;

        // 3. Shooting Stars
        ctx.strokeStyle = "#ffffff";
        ctx.lineWidth = 1;
        for (let i = shootingStars.length - 1; i >= 0; i--) {
            let s = shootingStars[i];

            ctx.beginPath();
            ctx.moveTo(s.x, s.y);
            ctx.lineTo(s.x - s.vx * 3, s.y - s.vy * 3);
            ctx.stroke();

            s.x += s.vx;
            s.y += s.vy;
            s.life -= 0.01;

            if (s.life <= 0 || s.x > width || s.y > height) {
                shootingStars.splice(i, 1);
            }
        }

        // 4. Draw Moon (Top-Left, Looping off-screen)
        const moonRadius = height * 0.6;
        const moonX = -moonRadius * 0.15;
        const moonY =
            -moonRadius * 0.15 + Math.sin(time * 0.5) * (height * 0.05);

        ctx.beginPath();
        ctx.arc(moonX, moonY, moonRadius, 0, Math.PI * 2);
        ctx.fillStyle = "#e0e0e0";
        ctx.fill();

        // 5. Draw Ocean
        ctx.fillStyle = "#808080";
        ctx.beginPath();
        ctx.moveTo(0, height);

        const waterLevel = height * 0.8;

        for (let x = 0; x <= width; x++) {
            const wave1 = Math.sin(x * 0.05 + time * 1.0) * 2;
            const wave2 = Math.sin(x * 0.1 - time * 0.5) * 1.5;
            const y = waterLevel + wave1 + wave2;
            ctx.lineTo(x, y);
        }

        ctx.lineTo(width, height);
        ctx.lineTo(0, height);
        ctx.fill();

        // 6. Ocean Reflection
        ctx.fillStyle = "#ffffff";
        const reflectionWidth = moonRadius * 0.6;
        ctx.globalAlpha = 0.2;
        ctx.fillRect(0, waterLevel + 2, reflectionWidth, height - waterLevel);
        ctx.globalAlpha = 1.0;
    }

    function renderAscii(ctx: CanvasRenderingContext2D) {
        if (width <= 0 || height <= 0) return;
        try {
            const imageData = ctx.getImageData(0, 0, width, height);
            const data = imageData.data;
            let output = "";

            for (let y = 0; y < height; y++) {
                for (let x = 0; x < width; x++) {
                    const offset = (y * width + x) * 4;
                    const r = data[offset];
                    const g = data[offset + 1];
                    const b = data[offset + 2];
                    const avg = (r + g + b) / 3;
                    const charIndex = Math.floor(
                        (avg / 255) * (DENSITY.length - 1),
                    );
                    output += DENSITY[charIndex];
                }
                output += "\n";
            }
            asciiText = output;
        } catch (e) {
            console.warn("Frame skip", e);
        }
    }

    function loop() {
        if (!canvasRef) return;
        const ctx = canvasRef.getContext("2d", { willReadFrequently: true });
        if (!ctx) return;

        time += 0.002;
        spawnShootingStar();
        drawScene(ctx);
        renderAscii(ctx);

        animationId = requestAnimationFrame(loop);
    }

    function handleResize() {
        if (!cardRef) return;

        const fontSize = 8;
        const charWidthApprox = fontSize * 0.6;

        const rect = cardRef.getBoundingClientRect();
        const newWidth = Math.ceil(rect.width / charWidthApprox);
        const newHeight = Math.ceil(rect.height / fontSize);

        if (width !== newWidth || height !== newHeight) {
            width = Math.max(1, newWidth);
            height = Math.max(1, newHeight);
            if (canvasRef) {
                canvasRef.width = width;
                canvasRef.height = height;
            }
            initStars();
        }
    }

    onMount(() => {
        handleResize();
        window.addEventListener("resize", handleResize);
        loop();
        return () => {
            window.removeEventListener("resize", handleResize);
            cancelAnimationFrame(animationId);
        };
    });

    const { class: cardClass, children } = $props();
</script>

<div class={`card ${cardClass}`} bind:this={cardRef}>
    <canvas bind:this={canvasRef} style="display: none;"></canvas>
    <div class="ascii-container">
        {asciiText}
    </div>
    <div class="scanlines"></div>

    <!-- Content Slot -->
    <div class="ui-layer">
        {@render children?.()}
    </div>
</div>

<style>
    .card {
        position: relative;
        width: 100%;
        max-width: 700px;
        height: 300px;
        background-color: #1a1a1a;
        border-radius: 0.5rem;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        overflow: hidden;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        border: 1px solid #e5e7eb;
    }

    .ascii-container {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        font-family: "Courier New", Courier, monospace;
        font-size: 8px;
        line-height: 8px;
        white-space: pre;
        overflow: hidden;
        z-index: 1;
        opacity: 0.7;
        pointer-events: none;
        background: linear-gradient(180deg, #ffffff 0%, #9ca3af 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }

    .ui-layer {
        position: relative;
        z-index: 10;
        text-align: center;
        color: white;
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        text-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
    }

    .scanlines {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: linear-gradient(
            to bottom,
            rgba(255, 255, 255, 0),
            rgba(255, 255, 255, 0) 50%,
            rgba(0, 0, 0, 0.1) 50%,
            rgba(0, 0, 0, 0.1)
        );
        background-size: 100% 4px;
        z-index: 5;
        pointer-events: none;
        opacity: 0.3;
    }
</style>
