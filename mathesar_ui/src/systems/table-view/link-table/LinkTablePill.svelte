<script lang="ts">
  import TableName from '@mathesar/components/TableName.svelte';
  import type { Table } from '@mathesar/models/Table';
  import { getTablePageUrl } from '@mathesar/routes/urls';

  export let table: Pick<Table, 'name'>;
  export let which: 'base' | 'target' | 'mapping';

  function isExistingTable(value: Pick<Table, 'name'>): value is Table {
    return 'schema' in value && 'oid' in value;
  }

  function getTablePageHref(value: Pick<Table, 'name'>): string | undefined {
    if (!isExistingTable(value)) {
      return undefined;
    }
    return getTablePageUrl(
      value.schema.database.id,
      value.schema.oid,
      value.oid,
    );
  }

  $: tablePageUrl = getTablePageHref(table);
</script>

{#if tablePageUrl}
  <a
    class="table-pill"
    class:base={which === 'base'}
    class:target={which === 'target'}
    class:mapping={which === 'mapping'}
    href={tablePageUrl}
  >
    <TableName
      {table}
      cssVariables={{ '--icon-color': 'var(--color-fg-base)' }}
    />
  </a>
{:else}
  <span
    class="table-pill"
    class:base={which === 'base'}
    class:target={which === 'target'}
    class:mapping={which === 'mapping'}
  >
    <TableName
      {table}
      cssVariables={{ '--icon-color': 'var(--color-fg-base)' }}
    />
  </span>
{/if}

<style>
  .table-pill {
    border-radius: 4px;
    padding: 0.25em 0.5em;
    margin: 0 0.25em;
    display: inline-flex;
    align-items: center;
    font-size: 0.9em;
    line-height: 1.2;
    color: var(--color-fg-base);
    font-weight: var(--font-weight-semibold);
    text-decoration: none;
  }

  .table-pill:hover,
  .table-pill:focus {
    text-decoration: underline;
  }

  .base {
    background: var(--base-fill);
    border: 1px solid var(--base-stroke);
  }

  .target {
    background: var(--target-fill);
    border: 1px solid var(--target-stroke);
  }

  .mapping {
    background: var(--mapping-fill);
    border: 1px solid var(--mapping-stroke);
  }
</style>
