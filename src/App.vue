<template>
  <div class="app">
    <div class="header">
      <strong>Документы</strong>

      <div class="header-buttons">
        <button class="header-button"><img class="img" src="@/components/img/favourite.svg"></button>
        <button class="header-button"><span><img class="img" src="@/components/img/plus.svg"></span><span>Новый тип</span></button>
        <button class="header-button"><span><img class="img" src="@/components/img/plus.svg"></span><span>Новый документ</span></button>
      </div>
    </div>

  <div class="text-field">
  <div class="text-field__icon">
    <span @click="searchValue(items, lists, searchQuery)" class="text-field__aicon">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16"><path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z" /></svg>
    </span>
    <input class="text-field__input" type="search" name="search" id="search" placeholder="Поиск" v-model="searchQuery">
  </div>
  <hr class=hr />
</div>
    
    
    <transition-group name="item-list">
      <div
        class="zone"
        v-for="list in lists.filter((l) => l.id !== 1000)"
        :key="list.id"
        @dragstart="onDragList($event, list, listDrop)"
        @drop="onDrop($event, list, listDrop)"
        :draggable="list.svoistvo"
        @dragover.prevent
        @dragenter="onDragEnter($event)"
        @dragleave="onDragLeave($event)"
        @dragend="onDragEnd($event)"
      >
        <div class="list-title">
          <div class="category">
            <button
              class="btn"
              @click="hiddenElem(list)"
            >
            <img class="img" src="@/components/img/open.svg">
            </button>

            <div class="list"><strong>{{ list.title }}</strong>
              <div class="dot-list"><div
                class="dot"
                :style="{ backgroundColor: dot.color }"
                v-for="dot in dots.filter((d) => d.list === list.id)"
                :key="dot.id"
              >
               <div class="dot-item">{{ dot.color }}</div> 
              </div></div>
              
              <span class="subtitle">{{ list.subTitle }}</span> <span class="text">{{ list.text }}</span> </div>
            
          </div>

          <div class="buttons-list">
            <button class="button-list">
             <img class="img" src="@/components/img/redact.svg">
            </button>
            <button class="button-list" @click="removeList(list.id)">
              <img class="img" src="@/components/img/delete.svg">
            </button>
            <button class="button-list" @click="changeDrag(list)">
              <img class="img" src="@/components/img/trans.svg">
            </button>
          </div>
        </div>

       


        <transition-group name="item-list">
          <div
            v-for="item in items.filter((x) => x.list === list.id)"
            :key="item.id"
            :draggable="item.svoistvo"
            @dragstart.stop="startDrag($event, item)"
            @drop="onDropItem($event, item, list)"
            @dragover.prevent
            @dragenter="onDragEnter($event)"
            @dragleave="onDragLeave($event)"
            @dragend="onDragEnd($event)"
          >
            <div class="item" v-if="list.isVisible">
              <div class="dotsspan"><span>{{ item.title }}</span>
              <div class="dot-list"><div
                class="dot"
                :style="{ backgroundColor: dot.color }"
                v-for="dot in dots.filter((d) => d.item === item.id)"
                :key="dot.id"
              >
               <div class="dot-item">{{ dot.color }}</div> 
              </div></div>

              <span class="subtitle">{{ item.subtitle }}</span>
              <span class="text">{{ item.text }}</span>

              </div>
              
              <div class="buttons-list">
                <button class="button-list">
                  <img class="img" src="@/components/img/redact.svg">
                </button>
                <button class="button-list" @click="removeItem(item.id)">
                  <img class="img" src="@/components/img/delete.svg">
                </button>
                <button class="button-list" @click="changeDragItem(item)">
                  <img class="img" src="@/components/img/trans.svg">
                </button>
              </div>
            </div></div
        ></transition-group></div
    ></transition-group>
          
    <div class="nocategory" v-for="list in lists.filter((c) => c.id === 1000)" :key="list.id"
      @drop="onDrop($event, list, listDrop)"
        :draggable="list.svoistvo"
        @dragover.prevent
        @dragenter.prevent>
      
        <transition-group name="item-list">
          <div
            v-for="item in itemsFilter.filter((x) => x.list === list.id)"
            :key="item.id"
            :draggable="item.svoistvo"
            @dragstart.stop="startDrag($event, item)"
            @drop="onDropItem($event, item, list)"
            @dragover.prevent
            @dragenter.prevent
          >
            <div class="item item__nocategory" v-if="list.isVisible">
              <div class="dotsspan"><span>{{ item.title }}</span>
              <div class="dot-list"><div
                class="dot"
                :style="{ backgroundColor: dot.color }"
                v-for="dot in dots.filter((d) => d.item === item.id)"
                :key="dot.id"
              >
               <div class="dot-item">{{ dot.color }}</div> 
              </div></div>

              <span class="subtitle">{{ item.subtitle }}</span>
              <span class="text">{{ item.text }}</span>

              </div>
              
              <div class="buttons-list">
                <button class="button-list">
                  <img class="img" src="@/components/img/redact.svg">
                </button>
                <button class="button-list" @click="removeItem(item.id)">
                  <img class="img" src="@/components/img/delete.svg">
                </button>
                <button class="button-list" @click="changeDragItem(item)">
                  <img class="img" src="@/components/img/trans.svg">
                </button>
              </div>
            </div></div
        ></transition-group>
    </div>
    


  </div>
</template>

<script>
import MyIcon from "@/components/MyIcon.vue";


export default {
  name: "App",
  data() {
    return {
      items: [
        { id: 0, title: "Паспорт", subtitle: "Обязательный", text: "Для всех", list: 1,},
        { id: 1, title: "ИНН", subtitle: "Обязательный", text: "Для всех", list: 1,},
        { id: 2, title: "Тестовое задание кандидата", subtitle: "", text: "Россия, Белоруссия, Украина, администратор филиала, повар-сушист, повар-пиццмейкер, повар горячего цеха", list: 1000,},
        { id: 3, title: "Трудовой договор", subtitle: "", text: "", list: 1000,},
        { id: 4, title: "Мед.книжка", subtitle: "", text: "", list: 1000,},
        
      ],
      lists: [
        {
          id: 1,
          title: "Обязательные для всех",
          subTitle: "",
          text: "Документы, обязательные для всех сотрудников без исключения",
          isVisible: true,
          order: 7,
        },
        {
          id: 2,
          title: "Обязательные для трудоустройства",
          subTitle: "",
          text: "Документы, без которых невозможно трудоустройство человека на какую бы то ни было должность в компании вне зависимости от граж",
          isVisible: true,
          order: 5,
        },
        {
          id: 3,
          title: "Специальные",
          subTitle: "",
          text: "",
          isVisible: true,
        },
        { id: 1000, title: "", isVisible: true },
      ],
      svoistvo: false,
      searchQuery: "",
      dots: [
        { id: 1, color: "#FF238D", list: 1 },
        { id: 2, color: "#FFB800", list: 1 },
        { id: 3, color: "#FF8D23", list: 1 },
        { id: 4, color: "#00C2FF", item: 0 },
        { id: 5, color: "#0066FF", item: 3 },
        { id: 6, color: "#8E9CBB", item: 3 },
      ],
    };
  },

  components: {
    MyIcon,
  },

  methods: {
    startDrag(evt, item, list) {
      console.log(evt.target.style)
      evt.target.style.opacity = "0.2"
      evt.dataTransfer.dropEffect = "move";
      evt.dataTransfer.effectAllowed = "move";
      evt.dataTransfer.setData("itemID", item.id.toString());
      evt.target.style.opacity = "0.5"
      
    },

    onDragList(evt, list, listDrop) {
      evt.dataTransfer.dropEffect = "move";
      evt.dataTransfer.effectAllowed = "move";
      evt.dataTransfer.setData("listDragID", list.id.toString());
    },

    onDragEnter(event) {
      event.preventDefault()
      event.target.style.borderBottom = '5px solid #0066FF'
      

    },

    onDragLeave(event) {
     
     event.target.style.borderBottom = ''
     
   },

   onDragEnd(event) {
     
     event.target.style.borderBottom = '1px solid #d3d8df'
     

   },


    onDrop(evt, listDrop, list) {
      const itemID = parseInt(evt.dataTransfer.getData("itemID"));
      var item = this.items.find((item) => item.id == itemID);
      const listDragID = parseInt(evt.dataTransfer.getData("listDragID"));

      var list = this.lists.find((list) => list.id == listDragID);

      let positionDrag;
      let listDrag;
      let positionDropList;
      if (list !== undefined) {
        for (let j = 0; j <= this.lists.length; j++) {
          list = this.lists[j];
          if (list.id == listDragID) {
            listDrag = list;
            positionDrag = j;
            break;
          }
        }

        for (let j = 0; j <= this.lists.length; j++) {
          list = this.lists[j];

          if (list.id == listDrop.id) {
            positionDropList = j;
            break;
          }
        }

        this.lists.splice(positionDrag, 1);
        this.lists.splice(positionDropList, 0, listDrag);
      } else {
        this.onDropItem(evt, item, listDrop);
      }
      evt.target.style.borderBottom = ''
      
    },

    compareNumeric(a, b) {
      if (a.order < b.order) return 1;
      if (a.order == b.order) return 0;
      if (a.order > b.order) return -1;
    },
    onDropItem(evt, itemDrop, list) {
     
      const itemID = parseInt(evt.dataTransfer.getData("itemID"));
      console.log(itemID);
      let item;
      let positionDrop;
      //const itemDrag = this.items.find((item) => item.id == itemID)
      let itemDrag;
      let position;

      //const itemDrag = this.items.find((item) => item.id == itemID)
      for (let i = 0; i <= this.items.length; i++) {
        const item = this.items[i];
        if (item.id == itemID) {
          itemDrag = item;
          position = i;
          break;
        } else {
          itemDrag = itemDrop;
          position = 0;
        }
      }

      for (let i = 0; i <= this.items.length; i++) {
        item = this.items[i];
        if (item.id == itemDrop.id) {
          positionDrop = i;
          break;
        }
      }

      if (itemDrag == undefined) {
        return;
      }
      itemDrag.list = list.id;
      this.items.splice(position, 1);
      this.items.splice(positionDrop, 0, itemDrag);
      console.log(this.items);
     

   
    },

    hiddenElem(list) {
      list.isVisible = !list.isVisible;
    },
    changeDrag(list) {
      list.svoistvo = !list.svoistvo;
    },
    changeDragItem(item) {
      item.svoistvo = !item.svoistvo;
    },

    removeList(id) {
      this.lists = this.lists.filter((l) => l.id != id);
    },

    removeItem(id) {
      this.items = this.items.filter((i) => i.id != id);
    },

    /*searchValue(items, lists, searchQuery) {
      let searchVal = this.lists.concat(items)
      var sortedSearchVal = searchVal.filter((item) => item.title.includes(this.searchQuery))
      console.log(sortedSearchVal)
    }*/




  },
  computed: {
    itemsFilter() {
      return this.items.filter((item) => item.title.includes(this.searchQuery));
    },

    listFilter() {
      return this.lists.filter((list) => list.title.includes(this.searchQuery));
    },
  },
};
</script>

<style>
* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  user-select: none;
 

 
}
#app {
  font-family: 'Fira Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;

  margin: 33px 30px 0px 30px;
  
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 22px;
  font-weight: 500;
  margin-bottom: 23px;
}

.header-buttons {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-button {
  display: inline-block;
  vertical-align: middle;
  background: white;
  border: 1px solid #d3d8df;
  border-radius: 50px;
  
  height: 30px;
  padding: 0px 20px;
  margin: 5px;
}

.text-field__icon {
  display: flex;
  vertical-align: middle;
  
}

.text-field__input {
  border: none;
  margin-left: 12px;
  outline: none;
  width: 50%;
}

.zone {
  min-height: 48px;
}

.list {
  display: flex;
  align-items: center;
  font-weight: 500;
  font-size: 15px;
  box-shadow: none;
}

.listA {
  box-shadow: 0 0 20px blue;
}

.item {
  border: 1px solid #d3d8df;
  margin-left: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 35px;
  
 
}



.item__nocategory {
margin: 0px;
}

.btn {
  
  height: 22px;
  width: 22px;
  background-color: white;
  border-radius: 50%;
  border: 1px solid #D3D8DF;
  cursor: pointer;
  margin: 13px 14px 13px 16px;
}

.btn img {
  align-items: center;
}

.item-list-move,
.item-list-enter-active,
.item-list-leave-active {
  transition: all 0.5s ease;
}
.item-list-enter-from,
.item-list-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}
.dotsspan {
  display: flex;
  align-items: center;
  margin-left: 15px;
  pointer-events: none;
  
}

.dot-list {
  display: flex;
  justify-content: space-between;
  margin: 0 13px;
}

.dot {
  background-color: color;
  border-radius: 50%;
  width: 8px;
  height: 8px;
  margin: 0 3px;
  
  
}

.dot-item {
  display:flex;
  justify-content: space-between;
  margin: 5px;
}

.list-title {
  display: flex;
  border: 1px solid #dfe4ef;
  background-color: white;
  align-items: center;
  height: 48px;
  justify-content: space-between;
  
}

.buttons-list {
  display: flex;
  justify-content: space-between;
}

.button-list {
  border:none;
  border-radius: 50%;
  background-color: white;
  margin: 0 5px;
  fill: blue;
  
}

.button-list svg {
  fill: #8e9cbb;
}

.btn img {
  fill: none;
  stroke: #1237a5;
  stroke-linecap: round;
  stroke-linejoin: round;
}

.search {
  margin-bottom: 19px;
  border: none;
}

.category {
  display: flex;
  align-items: center;
  pointer-events: none;
}

.img {
  height: 15px;
  width: 15px;
}

.dot div {
  display: none;
}

.nocategory {
  
  min-height: 50px;
  margin-top: 30px;
  margin-bottom: 50px;
}

.hr {
  width: 50%;
  margin-bottom: 19px;
}

.subtitle {
  font-size: 12px;
  color: #FF238D;
}

.text {
  font-size: 11px;
  color:#8e9cbb;
  margin-left: 16px;
}
</style>
