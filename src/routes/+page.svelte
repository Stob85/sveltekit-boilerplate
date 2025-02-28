<script>
	import { client } from '$lib/graphqlClient';
	import { gql } from 'graphql-tag';
	import { onMount } from 'svelte';
  
	let pros = [];
  
	const GET_PROS = gql`
	  query {
		pros(first: 10) {
		  nodes {
			id
			title
			allMeta {
			  key
			  value
			}
		  }
		}
	  }
	`;
  
	onMount(async () => {
	  const result = await client.query(GET_PROS).toPromise();
	  pros = result.data?.pros?.nodes || [];
	});
  </script>
  
  <h1>Liste des professionnels</h1>
  <ul>
	{#each pros as pro}
	  <li>
		<strong>{pro.title}</strong>
		<ul>
		  {#each pro.allMeta as meta}
			<li>{meta.key}: {meta.value}</li>
		  {/each}
		</ul>
	  </li>
	{/each}
  </ul>
  