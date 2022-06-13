<template>
  <div id="app">
    <vs-row>
    
      <vs-col vs-w="7">
        <vs-col vs-w="3" vs-offset="0.5" v-for="item in inventory" :key="item.name"
                vs-type="flex" vs-justify="left" vs-align="left">
          <vs-card type="3">
            Name: <b>{{item.name}}</b> <br>
            Description: <b>{{item.description}}</b> <br>
            Quantity: <b>{{item.quantity}}</b> <br>
            Price: <b>${{item.price}}</b> <br>
            Assigned Location: <b>{{item.location}}</b> <br><br>
            <vs-button color="danger" @click="deleteItem(item.name)">Delete Item</vs-button>
          </vs-card> 
        </vs-col>
      </vs-col>

      <vs-col vs-w="1">
        <vs-card>

          <vs-button type="line" @click="updateActionCard('Add Item')">
            Add New Item
          </vs-button>
          <br>
          <vs-button type="line" @click="updateActionCard('Edit Item')">
            Edit Item
          </vs-button>
          <br>
          <vs-button type="line" @click="updateActionCard('Add Warehouse')">
            Add Warehouse Location
          </vs-button>

        </vs-card>
      </vs-col>

       <vs-col vs-w="3" vs-offset="0.5">
         <vs-card>
           <b>{{selectedAction}}</b>
           <br>
           <br>

           <div v-if="selectedAction == 'Add Item'">
             <vs-input v-model="newItem.name" placeholder="Name(required)" />
             <vs-input v-model="newItem.description" placeholder="Description" />
             <vs-input v-model="newItem.quantity" placeholder="Quantity" type="number"/>
             <vs-input v-model="newItem.price" placeholder="Price (no symbol)" type="number"/>
             <vs-input v-model="newItem.location" placeholder="Location" />
             <br>
             <vs-button @click="addNewItem()">Add Item</vs-button>
           </div>

           <div v-if="selectedAction == 'Edit Item'">
             Select Item to edit:
             <select placeholder="Select" v-model="itemToEdit">
                <option v-for="item in inventory" :key="item.name" :label="item.name" :value="item.name">
                  {{item.name}}
                </option>
             </select>
             <vs-input v-model="newItem.description" placeholder="Description" />
             <vs-input v-model="newItem.quantity" placeholder="Quantity" type="number"/>
             <vs-input v-model="newItem.price" placeholder="Price (no symbol)" type="number"/>
             Location:
             <select placeholder="Select" v-model="newItem.location">
                <option v-for="warehouse in warehouses" :key="warehouse.location" :label="warehouse.location" :value="warehouse.location">
                  {{warehouse.location}}
                </option>
             </select>
             <br>
             <vs-button @click="editItem()">Edit Item</vs-button>
           </div>

           <div v-if="selectedAction == 'Add Warehouse'">
             <vs-input v-model="newWarehouse.location" placeholder="Location(required)" /> <br>
             <vs-button @click="addLocation()">Add Warehouse</vs-button> <br><br>
             Current Warehouses: <br>
             <p v-for="warehouse in this.warehouses" :key="warehouse.location">
               <b>{{warehouse.location}}</b> <vs-button color="danger" type="line" @click="deleteLocation(newWarehouse.location)">Delete</vs-button>
             </p>
           </div>
         </vs-card>
       </vs-col>

    </vs-row>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      inventory: [
        {'name': "Item #1", 'description': "Description for Item #1", 'quantity': 0, 'price': 100, 'location': "Berlin"},
        {'name': "Item #2", 'description': "Description for Item #2", 'quantity': 1, 'price': 150, 'location': "Belfast"},
        {'name': "Item #3", 'description': "Description for Item #3", 'quantity': 1, 'price': 100, 'location': "Barcelona"}
      ],
      warehouses: [
        {'location': "Berlin"},
        {'location': "Belfast"},
        {'location': "Barcelona"}
      ],
      selectedAction: "Add Item",
      newItem: {'name': "", 'description': "", 'quantity': 0, 'price': 0, 'location': ""},
      newWarehouse: {'location': ""},
      itemToEdit: ""
    }
  },
  methods: {
    updateActionCard(argument){
      this.selectedAction = argument;
    },
    addNewItem(){
      this.inventory.push(this.newItem);
      this.newItem = {'name': "", 'description': "", 'quantity': 0, 'price': 0, 'location': ""};
    },
    editItem(){
      for (var i = 0; i < this.inventory.length; i++) {
        if (this.inventory[i].name == this.itemToEdit) {
          if (this.newItem.name != "") {
            this.inventory[i].name = this.newItem.name;
          }
          if (this.newItem.description != "") {
            this.inventory[i].description = this.newItem.description;
          }
          if (this.newItem.quantity != "") {
            this.inventory[i].quantity = this.newItem.quantity;
          }
          if (this.newItem.price != "") {
            this.inventory[i].price = this.newItem.price;
          }
          if (this.newItem.location != "") {
            this.inventory[i].location = this.newItem.location;
          }
        }
      }
    },
    deleteItem(itemName) {
      for (var i = 0; i < this.inventory.length; i++) {
        if (this.inventory[i].name == itemName) {
          if (i > 0) {  
            this.inventory.splice(i, i);
          } else {
            this.inventory.shift()
          }
        }
      }
    },
    addLocation() {
      this.warehouses.push(this.newWarehouse);
      this.newWarehouse = {'location': ""};
    },
    deleteLocation(location) {
      for (var i = 0; i < this.warehouses.length; i++) {
        if (this.warehouses[i].location == location) {
          if (i > 0) {  
            this.warehouses.splice(i, i);
          } else {
            this.warehouses.shift()
          }
        }
      }
      for (i = 0; i < this.inventory.length; i++) {
        if (this.inventory[i].location == location) {
          this.inventory[i].location = "";
        }
      }
    }
  }
}


</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
