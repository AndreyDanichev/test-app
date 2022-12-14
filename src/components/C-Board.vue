<template>
  <div class="board">
    <div class="board-el" v-for="(el, idx) in data" :key="idx">
      <h3>{{ el.name }}</h3>
      <draggable
          class="list-group"
          group="tasks"
          v-model="el.list"
          v-bind="dragOptions"
          @start="drag = true"
          @end="drag = false"
      >
        <transition-group type="transition" :name="!drag ? 'flip-list' : null">
          <div
              class="list-group-item"
              v-for="element in el.list"
              :key="element.order"
              v-html="element.text"
          />
        </transition-group>
      </draggable>
      <textarea
          style="margin-bottom: 20px"
          name="card"
          id="new-card"
          v-model="newCardText"
          v-if="el.createCardState"
      />
      <button v-html="'Добавить карточку'" @click="addCard(el)" />
    </div>
    <button style="height: 50px" v-html="'Добавить столбец'" @click="addRow" />
    <c-modal ref="modal" @save-card="saveModalData" />
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: 'C-Board',
  components: {
    CModal: () => import('./C-Modal'),
    draggable
  },
  data () {
    return {
      data: [
        {
          name: 'В очереди',
          createCardState: false,
          list: [
            {
              order: 1,
              text: 'example card #1'
            },
            {
              order: 2,
              text: 'example card #2'
            }
          ]
        },
        {
          name: 'В работе',
          createCardState: false,
          list: [
            {
              order: 1,
              text: 'example card #1'
            },
            {
              order: 2,
              text: 'example card #2'
            }
          ]
        },
        {
          name: 'Сделано',
          createCardState: false,
          list: [
            {
              order: 1,
              text: 'example card #1'
            },
            {
              order: 2,
              text: 'example card #2'
            },
            {
              order: 3,
              text: 'example card #3'
            }
          ]
        }
      ],
      newCardText: '',
      drag: false
    }
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost"
      };
    }
  },
  methods: {
    addCard (el) {
      if (!el.createCardState) {
        this.data.map(e => e.createCardState = false)
        el.createCardState = true
      } else {
        if (this.newCardText.trim()) {
          el.list.push({
            order: el.list.length + 1,
            text: this.newCardText
          })
          el.createCardState = false
        }
      }
      this.newCardText = ''
    },
    saveModalData (data) {
      const elToAdd = {
        name: data.heading,
        createCardState: false,
        list: []
      }
      if (data.order > this.data.length) {
        this.data.push(elToAdd)
      } else {
        this.data.splice(data.order - 1, 0, elToAdd)
      }
    },
    addRow () {
      this.$refs.modal.openModal()
    }
  }
}
</script>

<style lang="scss" scoped>
.board {
  height: 100vh;
  display: flex;
  flex-flow: row nowrap;
  column-gap: 20px;
  overflow-y: auto;
  &-el {
    width: 300px;
    min-width: 300px;
    height: 100%;
    border: 1px solid #2c3e50;
  }
}
.list-group {
  display: grid;
  grid-auto-flow: row;
  grid-template-rows: 1fr;
  grid-row-gap: 10px;
  margin-bottom: 30px;
  span {
    display: flex;
    flex-flow: column nowrap;
    row-gap: 10px;
  }
  &-item {
    padding: 10px;
    border: 1px solid #2c3e50;
    margin: 0 5px;
    cursor: grab;
  }
}

.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
</style>