<template>
  <div class="levelComplete">
      <div class="insideClassText">
          👍 level complete in {{time}} ms!!
      </div>
   </div>
</template>

<script>
export default {
    props: [
        "time"
    ],
};
</script>

<style>
.levelComplete {
  position: fixed;
  font-size: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: deepskyblue;
  opacity: 0.7;
  z-index: 1;
  height: 100%;
  width: 100%;
  text-shadow: 0 0 3px azure;
}

.insideClassText{
    animation: pulse 1s infinite;
    transform: scale(1);
}

@keyframes pulse {
	0% {
		transform: scale(0.90);
	}

	70% {
		transform: scale(1);
	}

	100% {
		transform: scale(0.90);
	}
}
</style>
