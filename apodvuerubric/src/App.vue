<template>
  <section id="btnSect">
    <button @click="displayMain" class="btn open" id="mainBtn">
      Photo of the day
    </button>
    <button @click="displayDesc" class="btn closed" id="descBtn">
      Description
    </button>
  </section>
  <main id="main">
    <h1>{{ imgTitle }}</h1>
    <p>{{ imgDate }}</p>
    <img :src="imgUrl" :alt="imgTitle" id="mainImg" />
    <p>{{ imgAuthor }}</p>
  </main>
  <button @click="switchImg" id="leftArr" class="btn">previous</button>
  <button @click="returnImg" id="rightArr" class="hidden btn">next</button>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      APIkey: `7msuN5krwsbku6ppzLaaqfBjcEf2ojJUX9lzuwVW`,
      imgUrl: ``,
      imgDescription: ``,
      imgAuthor: ``,
      imgTitle: ``,
      imgDate: ``,
      imgUrlHD: ``,
      mediaType: ``,
    };
  },
  created() {
    fetch(`https://api.nasa.gov/planetary/apod?api_key=` + this.APIkey)
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
        this.imgUrlHD = data.hdurl;
        this.imgUrl = data.url;
        this.imgDescription = data.explanation;
        this.imgAuthor = data.copyright;
        this.imgTitle = data.title;
        this.imgDate = data.date;
        this.mediaType = data.media_type;
      })
      .catch((err) => console.log("Wrong: " + err));
  },
  methods: {
    displayDesc() {
      const mainBtn = document.getElementById("mainBtn");
      const descBtn = document.getElementById("descBtn");
      mainBtn.classList.add("closed");
      descBtn.classList.remove("closed");
      mainBtn.classList.remove("open");
      descBtn.classList.add("open");

      const main = document.getElementById("main");
      main.innerHTML = ``;

      const descH1 = document.createElement("h1");
      descH1.id = "newH1";
      descH1.innerText = this.imgTitle;
      main.appendChild(descH1);

      const descDiv = document.createElement("div");
      descDiv.id = "descDiv";
      main.appendChild(descDiv);

      const descImg = document.createElement("img");
      descImg.id = "descImg";
      descImg.src = this.imgUrl;
      descDiv.appendChild(descImg);

      const descTXT = document.createElement("div");
      descTXT.id = "descTXT";
      descTXT.innerText = this.imgDescription;
      descDiv.appendChild(descTXT);
    },
    displayMain() {
      const mainBtn = document.getElementById("mainBtn");
      const descBtn = document.getElementById("descBtn");
      mainBtn.classList.remove("closed");
      descBtn.classList.add("closed");
      mainBtn.classList.add("open");
      descBtn.classList.remove("open");

      const main = document.getElementById("main");
      main.innerHTML = ``;

      const title = document.createElement("h1");
      title.id = "title";
      title.innerText = this.imgTitle;
      main.appendChild(title);
      const date = document.createElement("p");
      date.id = "date";
      date.innerText = this.imgDate;
      main.appendChild(date);

      if (this.mediaType == "image") {
        const img = document.createElement("img");
        img.id = "mainImg";
        img.src = this.imgUrl;
        main.appendChild(img);
      }

      if (this.mediaType == "video") {
        const video = document.createElement("iframe ");
        video.id = "mainImg";
        video.src = this.imgUrl;
        main.appendChild(video);
      }

      const author = document.createElement("p");
      author.id = "author";
      author.innerText = this.imgAuthor;
      main.appendChild(author);
    },
    switchImg() {
      let newDate = new Date(this.imgDate);
      newDate.setDate(newDate.getDate() - 1);
      console.log(newDate);
      const dayEarlier = newDate.toISOString();
      console.log(dayEarlier);
      const dateArr = dayEarlier.split("T");
      const theDayBefore = dateArr[0];
      console.log(theDayBefore);

      fetch(
        `https://api.nasa.gov/planetary/apod?api_key=` +
          this.APIkey +
          `&date=` +
          theDayBefore
      )
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          this.imgUrlHD = data.hdurl;
          this.imgUrl = data.url;
          this.imgDescription = data.explanation;
          this.imgAuthor = data.copyright;
          this.imgTitle = data.title;
          this.imgDate = data.date;
          this.mediaType = data.media_type;
        });
      if (this.date != new Date()) {
        document.getElementById("rightArr").classList.remove("hidden");
      }
    },
    returnImg() {
      let newDate = new Date(this.imgDate);
      newDate.setDate(newDate.getDate() + 1);
      console.log(newDate);
      const dayLater = newDate.toISOString();
      console.log(dayLater);
      const dateArr = dayLater.split("T");
      const theDayLater = dateArr[0];
      console.log(theDayLater);

      fetch(
        `https://api.nasa.gov/planetary/apod?api_key=` +
          this.APIkey +
          `&date=` +
          theDayLater
      )
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          this.imgUrlHD = data.hdurl;
          this.imgUrl = data.url;
          this.imgDescription = data.explanation;
          this.imgAuthor = data.copyright;
          this.imgTitle = data.title;
          this.imgDate = data.date;
          this.mediaType = data.media_type;
        });
      if (this.date == "1995-06-15") {
        document.getElementById("leftArr").classList.add("hidden");
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.hidden {
  display: none;
}
body {
  background-color: #d4c1ec;
  color: #000000;
  margin: 0;
  padding: 0;
}

.btn {
  padding: 10px;
  border: none;
  border-radius: 5% 5% 0 0;
  margin: 50px 20px 0 0;
}

.btn:hover {
  cursor: pointer;
}

h1 {
  margin: 0 0 20px 0;
}

main {
  background-color: #000000;
  color: #d4c1ec;
  margin: 0;
  padding: 50px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

#mainImg {
  height: auto;
  width: 70vh;
}

#descImg {
  height: auto;
  width: 35vw;
  margin: 20px;
}

#descTXT {
  margin: 100px 20px;
}

#descDiv {
  display: flex;
}

.open {
  background-color: #000000;
  color: #d4c1ec;
}
.closed {
  background-color: #d4c1ec;
  color: #000000;
  border-top: #000000 solid 4px;
  border-left: #000000 solid 4px;
  border-right: #000000 solid 4px;
  box-sizing: border-box;
  padding: 6px;
}

#btnSect {
  margin: 0 20px 0 50px;
}
#rightArr {
  margin: 20px 20px 20px 90vw;
}
#leftArr {
  margin: 20px;
 
}
</style>
