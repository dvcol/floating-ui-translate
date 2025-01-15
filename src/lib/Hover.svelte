<script lang="ts">
  import { useDismiss, useFloating, useFocus, useHover, useInteractions } from '@skeletonlabs/floating-ui-svelte';
  import { scale } from 'svelte/transition';

  let triggerRef = $state<HTMLElement>()
  let ref = $state<HTMLElement>()

  let open = $state(false)
  const floating = useFloating({
    get elements() {
      return {
        floating: ref,
        reference: triggerRef,
      };
    },
    get open() {
      return open;
    },
    onOpenChange(_open) {
      open = _open;
    },
  });


  const _hover = useHover(floating.context, { move: false, delay: 100 });
  const _focus = useFocus(floating.context);
  const _dismiss = useDismiss(floating.context);
  const interactions = useInteractions([_hover, _focus, _dismiss]);

  $inspect(open)
</script>

<button bind:this={triggerRef} {...interactions.getReferenceProps()} >
  Hover Me
</button>


{#if floating.open}
  <div class="tooltip" bind:this={ref} transition:scale {...interactions.getFloatingProps()} style={floating?.floatingStyles}>
    <h1>Hello World</h1>
    <span>This is a tooltip</span>
  </div>
{/if}

<style>
  .tooltip {
    background-color: lavender;
    border: 1px solid black;
    color: black;
  }
</style>