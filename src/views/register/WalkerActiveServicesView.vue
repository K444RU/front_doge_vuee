<template>
  <div class="col row justify-content-center activeWalkingServiceBackGround">

    <div class="logo"><img style="margin-right: 1201px" alt="Vue logo" src="@/assets/img/doglogo.png"> </div>

    <WalkerButtonComponent/>

    <div class="col-lg-8" >
      <h1 style="color: floralwhite; font-style: oblique">Minu poolt avalikustatud teenused</h1>

      <div class="">
        <table class="table table-success table-striped tableShadow">
          <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Linn</th>
            <th scope="col">Kuupäev alates</th>
            <th scope="col">Kuupäev kuni</th>
            <th scope="col">Kellaaeg alates</th>
            <th scope="col">Kellaaeg kuni</th>
            <th scope="col">Koera suurus</th>
            <th></th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="walker in walkerTableInfoResponse">
            <th scope="row">{{ walker.sequenceNumber }}</th>
            <td>{{ walker.cityName }}</td>
            <td>{{ walker.dateFrom }}</td>
            <td>{{ walker.dateTo }}</td>
            <td>{{ walker.timeFrom }}</td>
            <td>{{ walker.timeTo }}</td>
            <td><div v-for="sizeType in walker.sizeTypes">
              {{ sizeType.sizeType }}
            </div></td>
            <td>
              <font-awesome-icon v-on:click="deleteService(walker.walkingId)" style="height: 35px" icon="fa-solid fa-trash"/>
            </td>
          </tr>
          </tbody>
        </table>

      </div>
    </div>

  </div>


</template>

<script>

import WalkerButtonComponent from "@/components/WalkerButtonComponent";

export default {
  name: "WalkerActiveServicesRoute",
  components: {WalkerButtonComponent},

  data: function () {
    return {
      userId: Number(sessionStorage.getItem('userId')),


      walkerTableInfoResponse: [
        {
          sequenceNumber: 0,
          walkingId: 0,
          cityName: '',
          dateFrom: '',
          dateTo: '',
          timeFrom: 0,
          timeTo: 0,
          sizeTypes: [
            {
              sizeType: ''
            }
          ]
        }
      ]
    }
  },
  methods: {
    getWalkerRegisteredServiceInfo: function () {

      this.$http.get("/walking", {
            params: {
              userId: Number(sessionStorage.getItem('userId')),
            }
          }
      ).then(response => {
        this.walkerTableInfoResponse = response.data
        this.addSequenceNumbers()
        console.log(response.data)
      }).catch(error => {
        console.log(error)
      })
    },
    addSequenceNumbers: function () {
      let counter = 1
      this.walkerTableInfoResponse.forEach(value => {
        value.sequenceNumber = counter
        counter++
      });
    },


    deleteService: function (walkingId) {
      this.$http.delete("/walking/delete", {
            params: {
              walkingId: walkingId
            }
          }
      ).then(() => {
        this.$emit(this.getWalkerRegisteredServiceInfo())
      }).catch(error => {
        console.log(error)
      })
    },


  },
  beforeMount() {
    this.getWalkerRegisteredServiceInfo()
  }
}
</script>

<style>

</style>