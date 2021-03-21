<template>
  <div class="container-characters">
    <div class="wrap-top">
      <div class="title">
        <p>무신사 과제</p>
        <hr>
      </div>
      <div class="filters">
        <button class="filter" @click="changeFilter('isAlive')" :class="{active: filters.isAlive}">
          생존인물만
        </button>
        <button class="filter" @click="changeFilter('isFemale')" :class="{active: filters.isFemale}">
          여자
        </button>
        <button class="filter" @click="changeFilter('hastvSeries')" :class="{active: filters.hastvSeries}">
          tvSeries 없음
        </button>
        <button class="reset" @click="resetCharacters">
          초기화
        </button>
      </div>
    </div>

    <div class="characters" v-if="characters.length > 0">
      <div class="character" v-for="(character, index) in characters" :key="index"  v-show="!character.is_deleted" :class="{notvSeries: filters.hastvSeries && (character.tvSeries.length == 0)}">
        <div class="information">
          <div>name: {{character.name}}   aliases: {{character.aliases.join(', ')}}</div>
          <div>
            title: {{character.title}}
          </div>
          <div>
            books: {{character.books.length}}   tvSeries: {{character.tvSeries.length}}
          </div>
        </div>
        <button class="btn-delete" @click="deleteCharacter(index)" >
          삭제
        </button>
      </div>
    </div>
    <div v-if="characters.length == 0 && !isLoading">
      no character list
    </div>
    <div class="scroll-observer" ref="scrollObserver"></div>    
    <div v-show="isLoading" class="default-spinner"></div>
  </div>
</template>

<script>
export default {
  name: 'CharacterList',

  data () {
    return {
      characters : [],
      page: 1,
      pageSize: 10,
      callCount: 0,
      isLoading: false,
      hasMore: true,
      filters: {
        isAlive: false,
        isFemale: false,
        hastvSeries: false
      }
    }
  },
  methods: {
    deleteCharacter: function (index) {
      this.characters[index].is_deleted = true;
    },
    resetCharacters: function () {
      this.characters.forEach(character => {
        character.is_deleted = false;
      });
    },
    getCharacters: async function(page, pageSize) {
      if(this.isLoading) {
        return;
      }
      this.isLoading = true;
      let requestUrl = `https://www.anapioficeandfire.com/api/characters?page=${page}&pageSize=${pageSize}`;
      if( this.filters.isAlive ) {
        requestUrl = requestUrl + '&isAlive=true';
      }
      if( this.filters.isFemale ) {
        requestUrl = requestUrl + '&gender=Female'
      }

      await this.$http.get(requestUrl)
      .then((result) => {
        let characters = result.data;
        this.callCount = this.callCount + 1;

        if( characters.length == 0 ) {
          this.hasMore = false;
        } else {
          characters.map(character => {
            character.is_deleted = false;
            return character;
          })

          if( this.characters.length == 0 ) {
            this.characters = characters;
          } else {
            this.characters = this.characters.concat(characters);
          }
        }
      })

      this.isLoading = false;
    },
    changeFilter(key) {
      this.filters[key] = !this.filters[key];
      if( key != 'hastvSeries') {
        this.characters = [];
        this.callCount = 0;
        this.getCharacters(this.page, this.pageSize);
      }
    },

    initIntersectionObserver: async function () {
      const io = new IntersectionObserver(async ([entry], observer) => {
      if (entry.isIntersecting) {
        observer.unobserve(entry.target);
        await this.getCharacters(this.page + this.callCount, this.pageSize);
        
        if(this.hasMore) {
          observer.observe(entry.target);
        }
      }
    }, {
      threshold: 1
    })
      io.observe(this.$refs.scrollObserver);
    },
    
  },
  mounted () {
    if( this.$route.query.page ) {
      this.page = this.$route.query.page;
    }
    if( this.$route.query.pageSize ) {
      this.pageSize = this.$route.query.pageSize;
    }
    this.initIntersectionObserver();
  }
}
</script>

<style scoped lang="scss">
$pri: #128CFC;
$sec: #fffffe;
$dark-pri: #0B5AA2;
$red-pri: #cc1111;
* {
  box-sizing: border-radius;
  font-family: 'Rubik', sans-serif;
}
.container {
  border: 1px solid black;
  
  position: absolute;
  top: 0; right: 0; bottom: 0; left: 0;
  margin: auto;
  
  display: grid;
  place-items: center;
  background-color: $pri;
}

.container-characters {
  background: $sec;
      
  .wrap-top {
    border-top: 10px solid $dark-pri;
    position:fixed;
    top:0;
    width:100%;
    background-color: white;

    .title p{
      padding: 5px 20px;
      margin: 10px;
      color: #0B5AA2;
      font-weight: bold;
      font-size: 20px;
    }

    .title hr {
      width: 20%;
      margin: 0px 30px;
      border: 1px solid $dark-pri;
    }

    .filters {
      display: flex;
      flex-flow: row nowrap;
      justify-content: space-around;
      margin-top: 20px;
      padding: 5px;

      .filter {
        padding: 8px 14px;
        border-radius: 10px;
        border: 1px solid $red-pri;
        background-color: white;

        &.active {
          border: 1px solid transparent;
          background-color:$red-pri;
          color: white;
        }
      }

      .reset {
        border: 1px solid transparent;
        background-color:$pri;
        color: white;
      }
    }
  }
  

  .characters {
    padding: 10px;
    margin: 10px;
    display: flex;
    flex-flow: column;
    padding-top: 120px;

    .character {
      padding: 10px;
      padding-right: 0px;
      display: flex;
      flex-flow: row nowrap;
      justify-content: space-between;
      align-items: center;
      // background-color: lightblue;
      font-size: 13px;
      border: 1px solid transparent;
      cursor: pointer;

      .information {
        display: flex;
        flex-flow: column;
        align-items: flex-start;
      }

      .btn-delete {
        font-size: 12px;
        padding: 10px 10px;
        height: 50%;
        margin-right: 10px;
        border-radius: 10px;
        background-color:$red-pri;
        border: 1px solid transparent;
        color:white;
      }

      &.notvSeries {
        display: none;
      }
    }
  }

  .scroll-observer {
    content: " ";
    height: 1px;
    width: 100%;
  }

  .character:hover {
    border-radius: 15px;
    border: 1px solid $dark-pri;
  }
  
}

.default-spinner {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.default-spinner:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #000;
  border-color: #000 transparent #000 transparent;
  animation: defaultSpinner 1.2s linear infinite;
}
@keyframes defaultSpinner {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}


</style>
