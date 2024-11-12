<script lang="ts">
  import { writable } from 'svelte/store';
  import {
    SvelteFlow,
    Controls,
    Background,
    BackgroundVariant,
    
    useSvelteFlow,
    Position,
    type NodeTypes,
    type Node,
    type Edge
  } from '@xyflow/svelte';

  import Sidebar from './Sidebar.svelte';
  import '@xyflow/svelte/dist/style.css';
  import { useDnD } from './utils';

  const nodes = writable([
  {
    id: 'horizontal-1',
    sourcePosition: Position.Right,
    type: 'input',
    data: { label: 'Inbound' },
    position: { x: 0, y: 0 }
  },
  {
    id: 'horizontal-2',
    sourcePosition: Position.Right,
    targetPosition: Position.Left,
    data: { label: 'Microservice' },
    position: { x: 200, y: 0 }
  },
  {
    id: 'horizontal-4',
    sourcePosition: Position.Right,
    targetPosition: Position.Left,
    data: { label: 'simple-psql' },
    position: { x: 400, y: 0 }
  }
  ]);

  const edges = writable([
  {
    id: 'horizontal-e1-2',
    source: 'horizontal-1',
    type: 'smoothstep',
    target: 'horizontal-2',
    animated: true
  },
  {
    id: 'horizontal-e1-4',
    source: 'horizontal-2',
    type: 'smoothstep',
    target: 'horizontal-4'
/*    label: 'edge label' */
  }
  ]);

  const { screenToFlowPosition } = useSvelteFlow();

  const type = useDnD();

  const onDragOver = (event: DragEvent) => {
    event.preventDefault();

    if (event.dataTransfer) {
      event.dataTransfer.dropEffect = 'move';
    }
  };

  const onDrop = (event: DragEvent) => {
    event.preventDefault();

    if (!$type) {
      return;
    }

    const position = screenToFlowPosition({
      x: event.clientX,
      y: event.clientY
    });

    const newNode = {
      id: `${Math.random()}`,
      type: $type,
      position,
      data: { label: `${type} node` },
      origin: [0.5, 0.0]
    } satisfies Node;

    $nodes.push(newNode);
    $nodes = $nodes;
  };
</script>

<main>
  <SvelteFlow {nodes} {edges} fitView on:dragover={onDragOver} on:drop={onDrop}>
    <Controls />
    <Background variant={BackgroundVariant.Dots} />

  </SvelteFlow>
  <Sidebar />
</main>

<style>
  main {
    height: 100vh;
    display: flex;
    flex-direction: column-reverse;
  }
</style>
