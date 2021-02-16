<script>
  import CanvasDraw from "../components/Canvas/CanvasDraw.svelte";
  let brushColor = "#ff0000";
  let brushRadius = 5;
  let bgImage = "../amsler.png";
  let imgBase64 = null;
  let SDraw = null;

  let alertOpen = true;

  function closeAlert() {
    alertOpen = false;
  }

  function setUploadedImage(e) {
    // console.log(fileUploader.files)
    console.log(e.target.files[0]);

    if (e.target.files[0]) {
      let reader = new FileReader();
      reader.readAsDataURL(e.target.files[0]);
      console.log(reader.result);
      reader.onload = (ev) => {
        // console.log(ev.target.result)
        // imgBase64 = reader.result;
        imgBase64 = ev.target.result;
        setBgImage();
      };
    }
  }

  function downloadDrawingFile(contentBase64) {
    const downloadLink = document.createElement("a");
    document.body.appendChild(downloadLink);

    downloadLink.href = contentBase64;
    downloadLink.target = "_self";
    downloadLink.download = "svelte-draw-export-" + +new Date();
    downloadLink.click();
  }

  function setBgImage() {
    bgImage = imgBase64;
  }

  function clear() {
    SDraw.clearDrawings();
  }

  function undo() {
    SDraw.undoDrawings();
  }

  function get_image_data() {
    let preparedDS = SDraw.get_image_data();
    downloadDrawingFile(preparedDS);
  }
</script>

<style>
  .circle-container {
    width: 100%;
    text-align: center;
  }
  .circle {
        margin-left: 200px;
        margin-top: 200px;
        border-radius: 50%;
        background-color: #ff0000;
        width: 150px;
        height: 150px;
        position: absolute;
        opacity: 1;
        animation: scaleIn 1.5s infinite cubic-bezier(.36, .11, .89, .32);
      }
      @keyframes scaleIn {
  from {
    transform: scale(.1, .1);
    opacity: 1;
  }
  to {
    transform: scale(.5, .5);
    opacity: 0;
  }
}
  </style>
  
<div class="container">
  <div class="container mx-auto">
    {#if alertOpen}
      <div
        v-if="alertOpen"
        class="text-white px-6 py-4 border-0 rounded relative mb-4 bg-orange-500"
      >
        <span class="text-xl inline-block mr-3 align-middle">
          <i class="fas fa-bell" />
        </span>
        <span class="inline-block align-middle mr-8">
          <b class="capitalize">Step 1:</b> Align your sight with the center of grid.
          Then, draw blurred region with the pentool.
        </span>
        <button
          class="absolute bg-transparent text-2xl font-semibold leading-none right-0 top-0 mt-4 mr-4 outline-none focus:outline-none"
          on:click={closeAlert}
        >
          <span>×</span>
        </button>
      </div>
    {/if}

    <div
      class="flex flex-wrap justify-center bg-white shadow-xl rounded-lg  py-16 px-12 relative z-10"
    >
      <div class="w-screen object-center">
          <div class="circle" />
        <CanvasDraw
          bind:this={SDraw}
          {brushColor}
          {brushRadius}
          imgSrc={bgImage}
          canvasWidth="550"
          canvasHeight="550"
        />
        <div class="sm:block flex flex-col mt-10">
          <button
            on:click={clear}
            class="github-star sm:ml-1 text-white font-bold px-6 py-4 rounded outline-none focus:outline-none mr-1 mb-1 bg-gray-800 active:bg-gray-700 uppercase text-sm shadow hover:shadow-lg ease-linear transition-all duration-150"
          >
            <i class="fas fa-trash text-lg mr-1" />
            <span>Clear</span>
          </button>

          <button
            on:click={get_image_data}
            class="github-star sm:ml-1 text-white font-bold px-6 py-4 rounded outline-none focus:outline-none mr-1 mb-1 bg-gray-800 active:bg-gray-700 uppercase text-sm shadow hover:shadow-lg ease-linear transition-all duration-150"
          >
            <i class="fas fa-download text-lg mr-1" />
            <span>Download</span>
          </button>

          <a
            href="https://github.com/creativetimofficial/notus-svelte?ref=ns-index"
            target="_blank"
            class="github-star sm:ml-1 text-white font-bold px-6 py-4 rounded outline-none focus:outline-none mr-1 mb-1 bg-gray-800 active:bg-gray-700 uppercase text-sm shadow hover:shadow-lg ease-linear transition-all duration-150"
          >
            <i class="fab fa-github text-lg mr-1" />
            <label>Brush Color</label>
            <input type="color" bind:value={brushColor} />
          </a>
        </div>
        <div class="text-center mt-16">
          <div class="form-row align-items-center">
            <div class="col-auto">
              <label>Brush Color</label>
              <input type="color" bind:value={brushColor} />
            </div>
            <div class="col-auto">
              <label>Brush Radius</label>
              <input type="number" bind:value={brushRadius} />
            </div>
            <div class="col-auto">
              <label>Upload an image as drawing background</label>
              <input type="file" on:change={setUploadedImage} />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
