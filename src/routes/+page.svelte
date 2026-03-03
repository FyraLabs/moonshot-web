<script>
    import AsciiHeader from "$lib/components/AsciiHeader.svelte";
    import GitHub from "$lib/components/icon/GitHub.svelte";
    import { Download, ChevronDown } from "@lucide/svelte";
    import { Button } from "$lib/components/ui/button";
    import * as Card from "$lib/components/ui/card/index.js";
    import * as DropdownMenu from "$lib/components/ui/dropdown-menu/index.js";
    import { ButtonGroup } from "$lib/components/ui/button-group/index.js";
    import { Separator } from "$lib/components/ui/separator";
</script>

<svelte:head>
    <title>Moonshot</title>
</svelte:head>

<div class="min-h-screen">
    <main class="container mx-auto px-4 py-8 max-w-4xl">
        <AsciiHeader class="mx-auto mb-8">
            <h1 class="text-5xl font-bold font-mono text-foreground">
                Moonshot
            </h1>
            <div class="flex flex-row gap-3 py-3">
                <Button
                    variant="default"
                    href="https://github.com/FyraLabs/moonshot/releases/latest"
                    target="_blank"
                    rel="noopener noreferrer"
                >
                    <Download />
                    Download
                </Button>
                <!-- <ButtonGroup>
                    <Button variant="default">
                        <Download />
                        Download
                    </Button>
                    <DropdownMenu.Root>
                        <DropdownMenu.Trigger>
                            {#snippet child({ props })}
                                <Button
                                    {...props}
                                    variant="default"
                                    class="px-2"
                                >
                                    <ChevronDown />
                                </Button>
                            {/snippet}
                        </DropdownMenu.Trigger>
                        <DropdownMenu.Content>
                            <DropdownMenu.Item>Linux</DropdownMenu.Item>
                            <DropdownMenu.Item>Windows</DropdownMenu.Item>
                            <DropdownMenu.Item>macOS</DropdownMenu.Item>
                        </DropdownMenu.Content>
                    </DropdownMenu.Root>
                </ButtonGroup> -->
                <Button
                    variant="outline"
                    href="https://github.com/FyraLabs/moonshot"
                    target="_blank"
                    rel="noopener noreferrer"
                >
                    <GitHub />
                    GitHub
                </Button>
            </div>
        </AsciiHeader>

        <div class="prose prose-gray max-w-none dark:prose-invert">
            <p class="text-xl text-muted-foreground text-center mb-8">
                A flashing tool you actually want to use.
            </p>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 not-prose mb-8">
                <Card.Root class="shadow-sm hover:shadow-md transition-shadow">
                    <Card.Header>
                        <Card.Title>Fast</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-muted-foreground">
                            Written in Go, Svelte, and Wails, Moonshot feels
                            snappy and flashes fast.
                        </p>
                    </Card.Content>
                </Card.Root>

                <Card.Root class="shadow-sm hover:shadow-md transition-shadow">
                    <Card.Header>
                        <Card.Title>Private</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-muted-foreground">
                            No telemetry, no tracking, no phoning home. Your
                            business stays your business.
                        </p>
                    </Card.Content>
                </Card.Root>

                <Card.Root class="shadow-sm hover:shadow-md transition-shadow">
                    <Card.Header>
                        <Card.Title>Cross-Platform</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-muted-foreground">
                            Works seamlessly on Linux, Windows, and macOS, for
                            one tool that runs anywhere.
                        </p>
                    </Card.Content>
                </Card.Root>

                <Card.Root class="shadow-sm hover:shadow-md transition-shadow">
                    <Card.Header>
                        <Card.Title>Modern</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-muted-foreground">
                            A beautifully simple interface that just works. No
                            more clunky, confusing flashing tools.
                        </p>
                    </Card.Content>
                </Card.Root>

                <!-- <Card.Root
                    class="shadow-sm hover:shadow-md transition-shadow md:col-span-2"
                >
                    <Card.Header>
                        <Card.Title>Distro Discovery</Card.Title>
                    </Card.Header>
                    <Card.Content>
                        <p class="text-muted-foreground">
                            Browse and download images directly (coming soon).
                        </p>
                    </Card.Content>
                </Card.Root> -->
            </div>
        </div>

        <Separator class="mt-16 mb-8" />
        <footer>
            <div class="text-center text-muted-foreground text-sm">
                <p>
                    Made by <a
                        href="https://fyralabs.com"
                        target="_blank"
                        rel="noopener"
                        class="text-primary">Fyra Labs</a
                    > with ❤️
                </p>
            </div>
        </footer>
    </main>
</div>
