<script>
    import AsciiHeader from "$lib/components/AsciiHeader.svelte";
    import GitHub from "$lib/components/icon/GitHub.svelte";
    import { Download } from "@lucide/svelte";
    import { Button } from "$lib/components/ui/button";
    import * as Card from "$lib/components/ui/card/index.js";
</script>

<svelte:head>
    <title>Moonshot - The Flashing Tool You Actually Want to Use</title>
</svelte:head>

<div class="min-h-screen bg-gray-50">
    <main class="container mx-auto px-4 py-8 max-w-4xl">
        <!-- ASCII Header with clean styling -->
        <AsciiHeader class="mx-auto mb-8">
            <h1 class="text-5xl font-bold font-mono text-gray-900">Moonshot</h1>
            <div class="flex flex-row gap-3 py-3">
                <Button variant="default" class="bg-blue-600 hover:bg-blue-700 text-white">
                    <Download />
                    Download
                </Button>
                <Button variant="outline" class="border-gray-300 text-gray-700 hover:bg-gray-50">
                    <GitHub />
                    GitHub
                </Button>
            </div>
        </AsciiHeader>

        <!-- Main content in markdown-like style -->
        <div class="prose prose-gray max-w-none">
            <p class="text-xl text-gray-600 text-center mb-8">
                The flashing tool you actually want to use.<br />
                Beautiful, fast, and designed for humans.
            </p>

            <h2 class="text-2xl font-semibold text-gray-900 mb-4">Why Moonshot?</h2>

            <p class="text-gray-700 mb-4">
                <strong>Built out of frustration.</strong> Existing flashing tools are bloated, slow, or just plain annoying. We set out to build something better—something you'd actually enjoy using.
            </p>

            <p class="text-gray-700 mb-6">
                <strong>Smart features coming.</strong> Select distro images directly from within the app. No more hunting through websites.
            </p>

            <p class="text-gray-700 mb-8">
                <strong>Made with love.</strong> From FyraLabs, the team behind tauOS. We build things because we want them to exist.
            </p>

            <h2 class="text-2xl font-semibold text-gray-900 mb-6">Features</h2>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 not-prose mb-8">
                <Card.Root class="bg-white border border-gray-200 shadow-sm hover:shadow-md transition-shadow">
                    <Card.Header>
                        <Card.Title class="text-gray-900">Native Performance</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-gray-600">Built with Go and Svelte—no bloated web wrappers slowing you down.</p>
                    </Card.Content>
                </Card.Root>

                <Card.Root class="bg-white border border-gray-200 shadow-sm hover:shadow-md transition-shadow">
                    <Card.Header>
                        <Card.Title class="text-gray-900">Privacy First</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-gray-600">No telemetry, no tracking, no phoning home. Your business stays your business.</p>
                    </Card.Content>
                </Card.Root>

                <Card.Root class="bg-white border border-gray-200 shadow-sm hover:shadow-md transition-shadow">
                    <Card.Header>
                        <Card.Title class="text-gray-900">Cross-Platform</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-gray-600">Works seamlessly on Linux, Windows, and macOS.</p>
                    </Card.Content>
                </Card.Root>

                <Card.Root class="bg-white border border-gray-200 shadow-sm hover:shadow-md transition-shadow">
                    <Card.Header>
                        <Card.Title class="text-gray-900">Modern Design</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-gray-600">Clean interface that doesn't get in your way.</p>
                    </Card.Content>
                </Card.Root>

                <Card.Root class="bg-white border border-gray-200 shadow-sm hover:shadow-md transition-shadow md:col-span-2">
                    <Card.Header>
                        <Card.Title class="text-gray-900">Distro Discovery</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-gray-600">Browse and download images directly (coming soon).</p>
                    </Card.Content>
                </Card.Root>
            </div>

            <h2 class="text-2xl font-semibold text-gray-900 mb-4">About</h2>

            <p class="text-gray-700">
                Moonshot is a beautiful cross-platform flashing tool by FyraLabs. Write ISOs, create bootable drives, and flash images with confidence.
            </p>
        </div>

        <!-- Simple footer -->
        <footer class="mt-16 pt-8 border-t border-gray-200">
            <div class="text-center text-gray-500 text-sm">
                <p>© 2024 FyraLabs • Made with ❤️ by the tauOS team</p>
            </div>
        </footer>
    </main>
</div>
