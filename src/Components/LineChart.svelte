<script>
  import { onMount } from "svelte";
  import * as d3 from "d3";

  let exitVelocity = 50;
  let launchAngle = 50;
  let hoverText = "";
  let hoverX = 0;
  let hoverY = 0;

  onMount(() => {
    drawDiamond();
  });

  function drawDiamond() {
    const width = 500;
    const height = 500;
    const margin = { top: 20, right: 20, bottom: 20, left: 20 };

    const svg = d3
      .select("#diamond")
      .attr("width", width)
      .attr("height", height)
      .append("g")
      .attr("transform", `translate(${margin.left},${margin.top})`);

    const fieldWidth = width - margin.left - margin.right;
    const fieldHeight = height - margin.top - margin.bottom;

    // Draw the green diamond
    svg
      .append("rect")
      .attr("x", fieldWidth / 8)
      .attr("y", fieldHeight / 8)
      .attr("width", (3 * fieldWidth) / 3)
      .attr("height", (3 * fieldHeight) / 3)
      .attr("fill", "green")
      .attr("transform", `rotate(225, ${fieldWidth / 2}, ${fieldHeight / 2})`);

    // Draw the tan diamond
    svg
      .append("rect")
      .attr("x", fieldWidth / 4)
      .attr("y", fieldHeight / 4)
      .attr("width", fieldWidth / 2)
      .attr("height", fieldHeight / 2)
      .attr("fill", "tan")
      .attr("transform", `rotate(45, ${fieldWidth / 2}, ${fieldHeight / 2})`);

    const baseSize = fieldWidth / 20;
    const halfBaseSize = baseSize / 2;

    // Draw bases (white diamonds with black outlines)
    const bases = [
      { x: fieldWidth / 2, y: fieldHeight / 4, label: "Second Base" }, // Top (second base)
      { x: 3 * fieldWidth / 4, y: fieldHeight / 2, label: "First Base" }, // Right (first base)
      { x: fieldWidth / 2, y: 3 * fieldHeight / 4, label: "Home Plate" }, // Bottom (home plate)
      { x: fieldWidth / 4, y: fieldHeight / 2, label: "Third Base" }, // Left (third base)
    ];

    bases.forEach((base) => {
      svg
        .append("rect")
        .attr("x", base.x - halfBaseSize)
        .attr("y", base.y - halfBaseSize)
        .attr("width", baseSize)
        .attr("height", baseSize)
        .attr("fill", "white")
        .attr("stroke", "black")
        .attr("transform", `rotate(45, ${base.x}, ${base.y})`)
        .on("mouseover", (event) => {
          hoverText = base.label;
          hoverX = base.x;
          hoverY = base.y - 20; // Adjust position above the base
        })
        .on("mouseout", () => {
          hoverText = "";
        });
    });
  }
</script>

<svg id="diamond"></svg>

<div class="slider-container">
  <label for="exitVelocity">Exit Velocity</label>
  <input type="range" id="exitVelocity" min="0" max="100" bind:value={exitVelocity}>
  <span>{exitVelocity}</span>
  
  <label for="launchAngle">Launch Angle</label>
  <input type="range" id="launchAngle" min="0" max="100" bind:value={launchAngle}>
  <span>{launchAngle}</span>
</div>

{#if hoverText}
  <div class="hover-box" style="left: {hoverX}px; top: {hoverY}px;">{hoverText}</div>
{/if}

<style>
  svg {
    display: block;
    margin: auto;
    margin-top: 50px;
  }
  
  .slider-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 20px;
  }
  
  .slider-container label {
    margin-top: 10px;
  }
  
  .slider-container input {
    margin: 5px 0;
  }
  
  .slider-container span {
    margin-bottom: 20px;
  }
  
  .hover-box {
    position: absolute;
    background-color: white;
    border: 1px solid black;
    padding: 5px;
    border-radius: 3px;
    pointer-events: none;
  }
</style>













