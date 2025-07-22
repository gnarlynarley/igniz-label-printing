<script lang="ts">
  function fileToImage(file: File) {
    return new Promise<HTMLImageElement>((resolve, reject) => {
      const image = document.createElement('img');

      image.onload = () => {
        resolve(image);
      };

      image.onerror = () => {
        reject(new Error('Could not load image'));
      };
    });
  }
  let files: FileList | null = $state(null);
  let imageFile = $derived(files?.[0] ?? null);
  let imageSrc = $derived(imageFile ? URL.createObjectURL(imageFile) : null);
</script>

<input type="file" bind:files />
{#if imageSrc}
  <div class="letter">
    <img src={imageSrc} alt="" />
  </div>
{/if}

<style>
  @media print {
    input {
      display: none;
      visibility: hidden;
    }
  }

  .letter {
    aspect-ratio: 6 / 4;
    background: white;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
</style>
