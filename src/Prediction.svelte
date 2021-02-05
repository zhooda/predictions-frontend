<script>
import App from "./App.svelte";
import { Col, Row, Button, Input, Form } from 'sveltestrap';
import { Diamonds } from "svelte-loading-spinners";
// var sleep = require('sleep');

	let src = {
		verbose: 'type a message!',
		prediction: '-10,000'
	};
	let text = '';
	let submitable;
	$: submitable = text.length > 0
	
	function getPrediction(t) {
		src = {
			prediction: ''
		}
		fetch(`http://box.zeeshan.wtf:6969/api/v1/predict?text=${text}&verbose=true`,
		{method: 'GET',
		mode: 'cors'})
			.then((response) => response.json())
			.then(json => {
				// sleep.sleep(2)
				src = json
			})
		.catch(err => {
			src = err
		})
	}

	function handleEnter(e) {
		if (e.keyCode === 13 && text.length > 0) {
			getPrediction("b")
		}
	}
</script>

<style>
	:global(.predict) {
		margin-left: 0;
		margin-right: 0;
		width: 100%
	}
</style>

<!-- <Form>	 -->
	<Row>
		<Col md='8' xs='8'>
			<Input type="text" bind:value={text} on:submit={getPrediction(text)} on:keyup={handleEnter} placeholder="bonkers"/>
		</Col>
		<Col md='4' xs='4'>
			<Button disabled={!submitable} color="danger" class="predict" on:click={getPrediction(text)}>predict</Button>
		</Col>
	</Row>
	<Row class="mt-5">
		<Col md='12' xs='12'>
			{#if src.prediction.length === 0}
			<!-- <h1>loading...</h1> -->
			<h1 align="center">
				<Diamonds size="60" color="#FF3E00" duration="1s"></Diamonds>
			</h1>
			{:else}
			<h1>{src.verbose}</h1>
			{/if}
		</Col>
	</Row>
	<Row>
		<Col md='12' xs='12'>
			{#if src.prediction.length === 0}
			<!-- <h3>loading...</h3> -->
			{:else}
			<h3>{src.prediction}</h3>
			{/if}
		</Col>
	</Row>
<!-- </Form> -->