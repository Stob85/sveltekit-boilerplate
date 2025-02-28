<script lang="ts">
	import { client } from '$lib/graphqlClient';
	import { gql } from 'graphql-tag';
	import { onMount } from 'svelte';
  
	type Pro = {
	  id: string;
	  title: string;
	};
  
	let pros: Pro[] = [];
  
	const GET_PROS = gql`
	  query {
		pros(first: 10) {
		  nodes {
			id
			title
		  }
		}
	  }
	`;
  
	onMount(async () => {
	  const result = await client.query(GET_PROS,{}).toPromise();
	  console.log('Résultat GraphQL:', result);
	  pros = result.data?.pros?.nodes || [];
	});
  </script>
  
  
  <h1>Liste des professionnels</h1>
  <ul>
	{#each pros as pro}
	  <li>
		<strong>{pro.title}</strong>
	  </li>
	{/each}
  </ul>
  