<template>
 <div>
  <h1>Space to View Standard Cakes and mark available or unavailable</h1>
    <table>
      <thead>
        <tr>
          <th>Cake ID</th>
          <th>Cake Availability Status</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="cake in listOfCakes" v-bind:key="cake.cake_id">
          <td>{{cake.cake_id}}</td>
          <td>{{getAvailabilityText(cake.availability)}}</td>
          <td>
                <button @click="updateCakeStatus(cake.cake_id)">Change Cake Availability</button>
          </td>
        </tr>  
      </tbody>    
    </table>
    <table>
        <thead>
            <tr>
                <th>Parameter</th>
                <th>Options</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Cake Name</td>
                <td>
                   <input id="cakeName" type="text" v-model="newStandardCake.title" />
                </td>
            </tr>
            <tr>
                <td>Cake Description</td>
                <td>
                   <input id="cakeDescription" type="text" v-model="newStandardCake.description" />
                </td>
            </tr>    
            <tr>
                <td>Cake Style</td>
                <td>
                    <label v-for="style in Styles" v-bind:key="style.id">
                        <input type="radio" name="param1" value="option1" v-model="newStandardCake.style"> {{style.name}}
                    </label>
                </td>
            </tr>
            <tr>
                <td>Flavor</td>
                <td>
                    <label v-for="flavor in Flavors" v-bind:key="flavor.id">
                        <input type="radio" name="param2" value="option1" v-model="newStandardCake.flavor"> {{ flavor.description }}
                    </label>
                </td>
            </tr>
            <tr>
                <td>Frosting</td>
                <td>
                    <label v-for="frosting in Frostings" v-bind:key="frosting.id">
                        <input type="radio" name="param3" value="option1" v-model="newStandardCake.frosting"> {{ frosting.description }}
                    </label>
                </td>
            </tr>
            <tr>
                <td>Image (url)</td>
                <td>
                    <input id="cakeImage" type="text" v-model="newStandardCake.image" />
                </td>
            </tr>
            <tr>
                <td>
                  <button @click="addStandardCake(newStandardCake)">Add New Cake to Standard Offerings</button>
                </td>
            </tr>    
            <!-- Add more parameter rows as needed -->
        </tbody>
    </table>    
 </div> 
</template>

<script>
import CakeService from '../services/CakeService';


export default {
    data() {
    return {
        listOfCakes : [],
        Styles: [
        { id: 1, name: 'Layer' },
        { id: 2, name: 'Sheet' },
        { id: 3, name: 'Cupckaes' }   
        ],
        Flavors : [],
        Frostings : [],
        newStandardCake : []
    }
    },
    created() {
        CakeService.getCakes()
        .then((response) => {
          this.listOfCakes = response.data
        })
        CakeService.getFlavors()
        .then((response) => {
        this.Flavors = response.data;
        });
        CakeService.getFrostings()
        .then((response) => {
        this.Frostings = response.data;
        }); 
    },
    methods:{
      
      updateCakeStatus(id) {
        CakeService.changeCakeAvailibility(id).then(window.location.reload())
      },
       getAvailabilityText(Availability) {
        return Availability ? 'Available' : 'Not Available';
      },
      addStandardCake(newStandardCake) {
        CakeService.addStandardCake(newStandardCake);
      }

      
          
    },
    } 

  
</script>

<style>

.all-actions {
  margin-bottom: 40px;
}

</style>