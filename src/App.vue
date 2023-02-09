<template>
  <div id="container">
    <div id="card">
      <img id="image" :src="src" />
      <div id="tools">
        <i @click="info" id="info-btn" class="fi fi-br-info"></i>
        <i @click="copy" id="copy-btn" class="fi fi-br-copy-alt"></i>
        <i @click="refresh" id="refresh-btn" class="fi fi-br-refresh"></i>
      </div>
    </div>
  </div>
</template>

<script>
  import Swal from 'sweetalert2'
  
  export default {
    data() {
      return {
        src: '',
        id: '',
        likes: '',
        views: '',
        comments: '',
        downloads: '',
        imageWidth: '',
        imageHeight: ''
      }
    },
    mounted() {
      let randomNumber = Math.floor(Math.random() * 3) + 1
      this.generateImage(randomNumber)
    },
    methods: {
      generateImage(page) {
      fetch(`https://pixabay.com/api/?safesearch=true&page=${page}&per_page=200&key=33305631-b1e87523ae6458f28243bf3df`)
        .then(res => res.json())
        .then(result => {
          let randomNumber = Math.floor(Math.random() * 199) + 0
          let imageUrl = result.hits[randomNumber].largeImageURL
          this.id = result.hits[randomNumber].id
          this.likes = result.hits[randomNumber].likes
          this.views = result.hits[randomNumber].views
          this.comments = result.hits[randomNumber].comments
          this.downloads = result.hits[randomNumber].downloads
          this.imageWidth = result.hits[randomNumber].imageWidth
          this.imageHeight = result.hits[randomNumber].imageHeight
          this.src = imageUrl
        })
      },
      info() {
        Swal.fire({
          position: 'center',
          icon: 'info',
          title: `Id: ${this.id}\nLikes: ${this.likes}\nViews: ${this.views}\nComments: ${this.comments}\nDownloads: ${this.downloads}\nImage width: ${this.imageWidth}\nImage height: ${this.imageHeight}`,
          showConfirmButton: false,
        })
      },
      copy() {
        navigator.clipboard.writeText(this.src)
        Swal.fire({
          position: 'top',
          title: 'Image url copied to clipboard',
          showConfirmButton: false,
          timer: 1000
        })
      },
      refresh() {
        let randomNumber = Math.floor(Math.random() * 3) + 1
        this.generateImage(randomNumber)
      }
    }
  }
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Kanit:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

* {
  box-sizing: border-box;
  font-family: 'Kanit', sans-serif;
}

body {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

#container {
  width: 100%;
  height: 100vh;
  padding: 20px;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  background: whitesmoke;
}

#swal2-title {
  font-size: 20px;
}

#card {
  width: 100%;
  border-radius: 10px;
  overflow: hidden;
  background: white;
  box-shadow: 0px 1px 2px 1px lightgray;
}

#image {
  width: 100%;
}

#tools {
  padding: 10px;
  display: flex;
  justify-content: space-around;
}

i {
  color: lightgray;
}

i:active {
  color: gray;
}

#info-btn {
  font-size: 20px;
}

#copy-btn {
  font-size: 20px;
}

#refresh-btn {
  font-size: 18px;
}
</style>