<script>
	export let mouseX;
	export let mouseY;

	let selectedElement;
	let svg;
	let offset;

	function getMousePosition(evt) {
	  var CTM = svg.getScreenCTM();
	  return {
	    x: (evt.clientX - CTM.e) / CTM.a,
	    y: (evt.clientY - CTM.f) / CTM.d
	  };
	}

	const start = e => {
	  selectedElement = e.target;
	  // get the svg parent element
	  svg = selectedElement.parentElement.parentElement;

	  offset = getMousePosition(e);
	  offset.x -= +selectedElement.getAttributeNS(null, "x");
	  offset.y -= +selectedElement.getAttributeNS(null, "y");
	};
	const move = e => {
	  if (selectedElement) {
	    const mousePos = getMousePosition(e);

	    mouseX = mousePos.x - offset.x;
	    mouseY = mousePos.y - offset.y;
	  }
	};
	const end = () => {
	  selectedElement = null;
	  offset = null;
	};
</script>

<svelte:window
	on:mousemove|preventDefault={move}
	on:mouseup|preventDefault={end}
/>
<g class="draggable" on:mousedown|preventDefault|stopPropagation={start}>
	<slot />
</g>
