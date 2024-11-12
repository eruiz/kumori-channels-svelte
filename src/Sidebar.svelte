<script lang="ts">
  import { useDnD } from './utils';

  const type = useDnD();

  const onDragStart = (event: DragEvent, nodeType: string) => {
    if (!event.dataTransfer) {
      return null;
    }

    type.set(nodeType);

    event.dataTransfer.effectAllowed = 'move';
  };
</script>

<aside>
  <div class="label">Marketplace Components</div>
  <div class="nodes-container">
    <div 
      class="input-node node"
      on:dragstart={(event) => onDragStart(event, 'input')}
      draggable={true}
    >
      inbound
    </div>
    <div 
      class="default-node node"
      on:dragstart={(event) => onDragStart(event, 'default')}
      draggable={true}
    >
      simple-psql
    </div>
    <div 
      class="output-node node"
      on:dragstart={(event) => onDragStart(event, 'output')}
      draggable={true}
    >
      simple-mongodb
    </div>
  </div>
  <div class="label">Drag a component and connect it to the microservice.</div>
</aside>

<style>
  aside {
    width: 100%;
    background: #f4f4f4;
    font-size: 12px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .label {
    margin: 1rem 0;
    font-size: 0.8rem;
  }

  .nodes-container {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .node {
    margin: 0.5rem;
    border: 1px solid #111;
    padding: 0.5rem 1rem;
    font-weight: 700;
    border-radius: 3px;
    cursor: grab;
    text-align: center;
    width: 100px;
  }
</style>
