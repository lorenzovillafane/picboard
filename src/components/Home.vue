<template>
  <main>
    <div id="instruction">
      <h1>PICBOARD</h1>
      <div id="instructorMain">
        <div id="addUrl">
          <h2>Agregar desde URL</h2>
          <input type="text" v-model="imageUrl" placeholder="URL de la imagen">
          <input type="text" v-model="imageCaption" placeholder="Caption">
          <button @click="addImageFromUrl">Agregar</button>
          <p style="color: red">{{ urlWarning }}</p>
        </div>
        <div id="addEquipo">
          <h2 >Agregar desde Equipo</h2>
          <input type="file" ref="fileInput" style="display: none" @change="handleFileChange">
          <button @click="openFileInput">Seleccionar Imagen</button>
          <input type="text" v-model="fileCaption" placeholder="Caption">
          <button @click="addImageFromFile">Subir Imagen</button>
          <p style="color: red">{{ fileWarning }}</p>
        </div>
      </div>
    </div>
    <div id="gallery">
      <figure v-for="(image, index) in images" :key="index">
        <img :src="image.src" :alt="image.alt">
        <figcaption>{{ image.caption }}</figcaption>
        <button @click="removeImage(index)">Eliminar</button>
      </figure>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      images: [],
      imageUrl: '',
      imageCaption: '',
      fileCaption: '',
      urlWarning: '',
      fileWarning: '',
      selectedFile: null,
      selectedFileUrl: ''
    };
  },
  created() {
    // Al cargar el componente, intenta cargar las imágenes desde el almacenamiento local
    const storedImages = localStorage.getItem('images');
    if (storedImages) {
      this.images = JSON.parse(storedImages);
    }
  },
  watch: {
    // Vigilar cambios en la lista de imágenes y guardar en almacenamiento local
    images: {
      handler() {
        localStorage.setItem('images', JSON.stringify(this.images));
      },
      deep: true
    }
  },
  methods: {
    removeImage(index) {
      this.images.splice(index, 1);
    },
    addImageFromUrl() {
      if (this.imageUrl.trim() !== '' && this.imageCaption.trim() !== '') {
        this.images.push({
          src: this.imageUrl.trim(),
          alt: 'Custom Image',
          caption: this.imageCaption.trim()
        });
        this.imageUrl = ''; // Clear input after adding image
        this.imageCaption = ''; // Clear input after adding image
        this.urlWarning = ''; // Clear warning message
      } else {
        this.urlWarning = 'Please provide both URL and caption';
      }
    },
    handleFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        this.selectedFile = file;
      }
    },
    openFileInput() {
      this.$refs.fileInput.click();
    },
    addImageFromFile() {
      if (this.selectedFile && this.fileCaption.trim() !== '') {
        const reader = new FileReader();
        reader.onload = () => {
          this.images.push({
            src: reader.result,
            alt: 'Custom Image',
            caption: this.fileCaption.trim()
          });
          this.fileCaption = ''; // Clear input after adding image
          this.fileWarning = ''; // Clear warning message
        };
        reader.readAsDataURL(this.selectedFile);
        this.selectedFile = null; // Clear selected file
      } else {
        this.fileWarning = 'Please provide both a file and caption';
      }
    }
  }
};
</script>

  
  <style>
  @import url("https://fonts.googleapis.com/css2?family=Kalam:wght@400&display=swap");
  :root {
      --adjust-size: 0px; /* Change as needed */
  }
  * {font-family: "Kalam", sans-serif;
      margin: 0;
      padding: 0;
      box-sizing: border-box;
  }
#instruction{display: flex;
justify-content: center;
flex-direction: column;
align-items:center;
text-align: center;

}
#instructorMain{
  display: flex;
  justify-content: space-around;
  flex-direction: row;
}
#addUrl input{margin: 15px; padding: 5px;
  
}
h1{font-size: 60px;}
h2{font-size: 30px;}
#addUrl button{padding: 5px;
margin-right: 25px;}
#addEquipo input{margin: 15px;
  padding: 5px;
  
}
#addEquipo button{padding: 5px;}

  html,
  body {
      overscroll-behavior-x: none;
      overscroll-behavior-y: none;
      scroll-behavior: smooth;
  }
  body {
  	position: relative;
	color: #222;
	min-height: 100vh;
	overflow-x: hidden;
	background-image: url("https://images.unsplash.com/photo-1531685250784-7569952593d2?crop=entropy&cs=srgb&fm=jpg&ixid=M3wzMjM4NDZ8MHwxfHJhbmRvbXx8fHx8fHx8fDE2OTMyOTE2OTh8&ixlib=rb-4.0.3&q=100&w=3000");
	background-size: cover;
  }
  main {
      position: relative;
      display: flex;
      flex-direction: column;
      align-items: center;
      max-width: 100vw;
      min-height: 100vh;
      overflow-x: hidden;
  }
  p {
      line-height: 1;
  }
  a {
      color: crimson;
      text-decoration: none;
  }
  img {
      -moz-user-select: none;
      -webkit-user-select: none;
      -ms-user-select: none;
      user-select: none;
      pointer-events: none;
  }
  
  #gallery {
      position: relative;
      left: calc(-1 * var(--adjust-size));
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      gap: 20px;
      max-width: 100vw;
      padding: 20px;
      -webkit-perspective: 0;
      perspective: 0;
  }
  #gallery figure:nth-child(7n) {
      --duration: 1s;
      --pin-color: crimson;
  }
  #gallery figure:nth-child(7n + 1) {
      --duration: 1.8s;
      --pin-color: hotpink;
  }
  #gallery figure:nth-child(7n + 2) {
      --duration: 1.3s;
      --pin-color: magenta;
  }
  #gallery figure:nth-child(7n + 3) {
      --duration: 1.5s;
      --pin-color: orangered;
  }
  #gallery figure:nth-child(7n + 4) {
      --duration: 1.1s;
      --pin-color: darkorchid;
  }
  #gallery figure:nth-child(7n + 5) {
      --duration: 1.6s;
      --pin-color: deeppink;
  }
  #gallery figure:nth-child(7n + 6) {
      --duration: 1.2s;
      --pin-color: mediumvioletred;
  }
  #gallery figure:nth-child(3n) {
      --angle: 3deg;
  }
  #gallery figure:nth-child(3n + 1) {
      --angle: -3.3deg;
  }
  #gallery figure:nth-child(3n + 2) {
      --angle: 2.4deg;
  }
  #gallery figure:nth-child(odd) {
      --direction: alternate;
  }
  #gallery figure:nth-child(even) {
      --direction: alternate-reverse;
  }
  #gallery figure {
      --angle: 3deg;
      --count: 5;
      --duration: 1s;
      --delay: calc(-0.5 * var(--duration));
      --direction: alternate;
      --pin-color: red;
  
      position: relative;
      display: inline-block;
      margin: var(--adjust-size);
      padding: 0.5rem;
      border-radius: 5px;
      box-shadow: 0 7px 8px rgba(0, 0, 0, 0.4);
      width: 100%;
      height: auto;
      text-align: center;
      background-color: ghostwhite;
      background-image: url("https://images.unsplash.com/photo-1629968417850-3505f5180761?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=M3wzMjM4NDZ8MHwxfHJhbmRvbXx8fHx8fHx8fDE2OTMzMjQ3ODJ8&ixlib=rb-4.0.3&q=80&w=500");
      background-size: cover;
      background-position: center;
      background-blend-mode: multiply;
  
      transform-origin: center 0.22rem;
      will-change: transform;
      break-inside: avoid;
      overflow: hidden;
      outline: 1px solid transparent;
      -webkit-backface-visibility: hidden;
      backface-visibility: hidden;
  }
  #gallery.active figure {
      animation-duration: var(--duration), 1.5s;
      animation-delay: var(--delay),
          calc(var(--delay) + var(--duration) * var(--count));
      animation-timing-function: ease-in-out;
      animation-iteration-count: var(--count), 1;
      animation-direction: var(--direction), normal;
      animation-fill-mode: both;
      animation-name: swing, swingEnd;
  }
  #gallery figure:after {
      position: absolute;
      top: 0.22rem;
      left: 50%;
      width: 0.7rem;
      height: 0.7rem;
      content: "";
      background: var(--pin-color);
      border-radius: 50%;
      box-shadow: -0.1rem -0.1rem 0.3rem 0.02rem rgba(0, 0, 0, 0.5) inset;
      filter: drop-shadow(0.3rem 0.15rem 0.2rem rgba(0, 0, 0, 0.5));
      transform: translateZ(0);
      z-index: 2;
  }
  figure img {
      aspect-ratio: 1 /1;
      width: 100%;
      object-fit: cover;
      display: block;
      border-radius: 5px;
      margin-bottom: 10px;
      z-index: 1;
  }
  figure figcaption {
      font-size: 14px;
      font-weight: 400;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      z-index: 1;
  }
  figure h2 {
      color: crimson;
      font-size: 22px;
  }
  figure p {
      font-size: 17px;
  }
  figure small {
      font-size: 12px;
  }
  figure > div {
      width: 100%;
      height: 100%;
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
  }
  @keyframes swing {
      0% {
          transform: rotate3d(0, 0, 1, calc(-1 * var(--angle)));
      }
      100% {
          transform: rotate3d(0, 0, 1, var(--angle));
      }
  }
  @keyframes swingEnd {
      to {
          transform: rotate3d(0, 0, 1, 0deg);
      }
  }
  #info {
      position: relative;
      text-align: center;
      z-index: 1;
  }
  #info a {
      font-size: 1.1rem;
  }
 @media (max-width: 980px) {
#instructionMain{display: flex;
justify-content: space-around;}
#addUrl{display: flex;
  flex-direction: column; 
margin: 15px;}
#addUrl button{margin: 15px;}
#addEquipo{display: flex;
  flex-direction: column;
margin: 15px;}
#addEquipo button{margin: 15px;}


 }
  @media (min-width: 800px) {
      #gallery {
          grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      }
  }
  @media (max-width: 480px) {
h1{font-size: 50px;}
h2{font-size: 20px;}
input{width: 150px;}

 }
 @media (max-width: 420px) {
h1{font-size: 40px;}
h2{font-size: 15px;}
input{width: 120px;}

 }
 
  </style>
  