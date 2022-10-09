<template>
  <div class="container pt-6">
    <v-text-field
        outlined
        label="Add item"
        v-model="newItemTitle"
        clearable
        append-icon="mdi-basket-plus"
        @click:append="addNewItem()"
        @keyup.enter="addNewItem()"
    ></v-text-field>
    <v-list
        flat
    >
      <v-list-item-group
          v-for="(item) in shoppingList"
          :key="item.id"
      >
        <v-list-item
            @click="doneBought(item.id)"
            :class="{ 'blue lighten-5' : item.bought }"
        >
          <template>
            <v-list-item-action>
              <v-checkbox
                  :input-value="item.bought"
                  color="primary"
              ></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title :class="{'text-decoration-line-through' : item.bought}">
                {{item.title}}
              </v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn icon @click.stop="deleteItem(item.id)">
                <v-icon color="grey lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </v-list-item-group>
    </v-list>
  </div>
</template>

<script>
  export default {
    name: 'ListView',
    data: () => ({
      shoppingList: [
        {
          id: 1,
          title: "Juice",
          bought: false,
        },
        {
          id: 2,
          title: "Salt",
          bought: false,
        },
        {
          id: 3,
          title: "Pasta",
          bought: false,
        },
      ],
      newItemTitle: "",
    }),
    methods: {
      doneBought(id) {
        let item = this.shoppingList.filter(i => i.id === id)[0];
        item.bought = !item.bought;
      },
      deleteItem(id) {
        this.shoppingList = this.shoppingList.filter(i => i.id !== id);
      },
      addNewItem() {
        if (this.newItemTitle.trim() !== ""){
          let maxId = Math.max.apply(null, this.shoppingList.map(item => item.id));
          let item = {
            id: maxId+1,
            bought: false,
            title: this.newItemTitle,
          }
          this.shoppingList.push(item);
          this.newItemTitle=""
        }
      }
    }
  }
</script>
