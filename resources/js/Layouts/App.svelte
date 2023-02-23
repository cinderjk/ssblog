<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    let main;
    export let scroller = {
        scroll: 0,
        direction: true,
        delta: 0,
        hide: false,
        initialScroll: 0,
    };
    export let header = false;
    export let headerHeight = 64;
    export let hideHeader = false;
    export let footer = false;
    export let footerHeight = 64;
    export let hideFooter = false;
    export let threshold = 64;
    const Scroll = (e) => {
        const scroll = e.target.scrollTop;

        scroller = {
            scroll,
            direction: scroll > scroller.scroll,
            delta: scroll - scroller.initialScroll,
            hide:
                scroll > threshold && scroller.hide
                    ? scroll - scroller.initialScroll > -threshold
                    : scroll - scroller.initialScroll > threshold,
            initialScroll:
                scroll > scroller.scroll != scroller.direction
                    ? scroll
                    : scroller.initialScroll,
        };

        dispatch("scroller", scroller);
    };
    $: if (main) {
        main.style.setProperty("--header-height", headerHeight + "px");
        main.style.setProperty("--footer-height", footerHeight + "px");
    }
</script>

<div class="slotHeader" class:hide={hideHeader && scroller.hide}>
    <nav
        class="top-0 absolute z-50 w-full flex flex-wrap items-center justify-between px-2 py-3 "
    >
        <div
            class="container px-4 mx-auto flex flex-wrap items-center justify-between"
        >
            <div
                class="w-full relative flex justify-between lg:w-auto lg:static lg:block lg:justify-start"
            >
                <a
                    class="text-sm font-bold leading-relaxed inline-block mr-4 py-2 whitespace-nowrap uppercase text-white"
                    href="https://www.creative-tim.com/learning-lab/tailwind-starter-kit#/presentation"
                >
                    <slot name="header" />
                </a><button
                    class="cursor-pointer text-xl leading-none px-3 py-1 border border-solid border-transparent rounded bg-transparent block lg:hidden outline-none focus:outline-none"
                    type="button"
                >
                    <i class="text-white fas fa-bars" />
                </button>
            </div>
            <div
                class="lg:flex flex-grow items-center bg-white lg:bg-transparent lg:shadow-none hidden"
                id="example-collapse-navbar"
            >
                <ul class="flex flex-col lg:flex-row list-none mr-auto">
                    <li class="flex items-center">
                        <a
                            class="lg:text-white lg:hover:text-gray-300 text-gray-800 px-3 py-4 lg:py-2 flex items-center text-xs uppercase font-bold"
                            href="https://www.creative-tim.com/learning-lab/tailwind-starter-kit#/landing"
                            ><i
                                class="lg:text-gray-300 text-gray-500 far fa-file-alt text-lg leading-lg mr-2"
                            />
                            Docs</a
                        >
                    </li>
                </ul>
                <ul class="flex flex-col lg:flex-row list-none lg:ml-auto">
                    <li class="flex items-center">
                        <a
                            class="lg:text-white lg:hover:text-gray-300 text-gray-800 px-3 py-4 lg:py-2 flex items-center text-xs uppercase font-bold"
                            href="#pablo"
                            ><i
                                class="lg:text-gray-300 text-gray-500 fab fa-facebook text-lg leading-lg "
                            /><span class="lg:hidden inline-block ml-2"
                                >Share</span
                            ></a
                        >
                    </li>
                    <li class="flex items-center">
                        <a
                            class="lg:text-white lg:hover:text-gray-300 text-gray-800 px-3 py-4 lg:py-2 flex items-center text-xs uppercase font-bold"
                            href="#pablo"
                            ><i
                                class="lg:text-gray-300 text-gray-500 fab fa-twitter text-lg leading-lg "
                            /><span class="lg:hidden inline-block ml-2"
                                >Tweet</span
                            ></a
                        >
                    </li>
                    <li class="flex items-center">
                        <a
                            class="lg:text-white lg:hover:text-gray-300 text-gray-800 px-3 py-4 lg:py-2 flex items-center text-xs uppercase font-bold"
                            href="#pablo"
                            ><i
                                class="lg:text-gray-300 text-gray-500 fab fa-github text-lg leading-lg "
                            /><span class="lg:hidden inline-block ml-2"
                                >Star</span
                            ></a
                        >
                    </li>
                    <li class="flex items-center">
                        <button
                            class="bg-white text-gray-800 active:bg-gray-100 text-xs font-bold uppercase px-4 py-2 rounded shadow hover:shadow-md outline-none focus:outline-none lg:mr-1 lg:mb-0 ml-3 mb-3"
                            type="button"
                            style="transition: all 0.15s ease 0s;"
                        >
                            <i class="fas fa-arrow-alt-circle-down" /> Download
                        </button>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</div>
<main bind:this={main} class:header class:footer on:scroll={Scroll}>
    <slot {scroller} />
</main>
<div>
    <slot name="footer" />
</div>
