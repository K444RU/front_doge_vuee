<template>
  <div class="col row justify-content-center addWalkingServiceBackGround">

    <div class="logo"><img style="margin-right: 1201px" alt="Vue logo" src="@/assets/img/doglogo.png"> </div>

    <WalkerButtonComponent/>


    <div>
      <h1  style="color: floralwhite; margin-left: 75px">Leidke ideaalne sobivus</h1>
      <br>
      <br>
    </div>


    <div class="col-lg-2">
      <h3 style="color: black; font-size: xxx-large">Linn</h3>
      <select v-model="walkingRequest.cityId" class="form-select" aria-label="Default select example">
        <option selected disabled value="0">--Valige Linn--</option>
        <option v-for="city in cities" :value="city.cityId">{{ city.cityName }}</option>

      </select>
    </div>


    <div class="col-lg-2">
      <h3 style="color: black; font-size: xxx-large">Kuupäev</h3>
      <p>
        <label style="color: black" class="fw-bold" for="date">Alates</label>
        <input  v-model="walkingRequest.dateFrom" type="date" id="date">
      </p>
      <p>
        <label style="color: black" class="fw-bold" for="date">Kuni</label>
        <input v-model="walkingRequest.dateTo" type="date" id="date">
      </p>
    </div>


    <div class="col-lg-2">
      <h3 style="color: black; font-size: xxx-large">Kellaaeg</h3>
      <div  class="input-group mb-3">
        <span  class="input-group-text" id="basic-addon1">Alates</span>
        <input v-model="walkingRequest.timeFrom" type="number" class="form-control" placeholder="kellaaeg"
               aria-label="Username" aria-describedby="basic-addon1">
      </div>
      <div class="input-group mb-3">
        <span class="input-group-text" id="basic-addon1">Kuni</span>
        <input v-model="walkingRequest.timeTo" type="number" class="form-control" placeholder="kellaaeg"
               aria-label="Username" aria-describedby="basic-addon1">
      </div>


    </div>


    <div class="col-lg-4">
      <h3 style="font-size: xxx-large">Vali koera suurust</h3>
      <div v-for="size in walkingRequest.sizes" class="form-check">
        <input v-model="size.isSelected" class="form-check-input" type="checkbox" id="flexCheckDefault">
        <label class="form-check-label" for="flexCheckDefault">
          {{ size.sizeType }}
        </label>
      </div>

<!--      <div v-for="option in atmOptions" class="form-check">-->
<!--        <input v-model="option.isSelected" class="form-check-input" type="checkbox" id="flexCheckDefault">-->
<!--        <label class="form-check-label" for="flexCheckDefault">-->
<!--          {{ option.optionName }}-->
<!--        </label>-->
<!--      </div>-->

    </div>


    <div class="col col-lg-1">
      <br>
      <br>
      <!--      <button v-on:click="$router.push('/active/service')" type="button" class="btn btn-success">Lisa teenus</button>-->
<!--      <button v-on:click="addNewService" type="button" class="btn btn-success">Lisa uus teenus</button>-->
      <font-awesome-icon  v-on:click="addNewService" style="height: 75px; color: black;" icon="fa-solid fa-circle-plus" />
    </div>



  </div>
</template>

<script>

import WalkerButtonComponent from "@/components/WalkerButtonComponent";

export default {
  name: "AddWalkingServiceView",
  components: {WalkerButtonComponent},

  data: function () {
    return {
      cities:
          {
            cityId: '',
            cityName: ''
          },

      sizes:
          [
            {
              sizeId: 0,
              sizeType: '',
              isSelected: true
            }
          ],
      walkingRequest:
          {
            userId: Number(sessionStorage.getItem('userId')),
            cityId: 0,
            dateFrom: '',
            dateTo: '',
            timeFrom: '',
            timeTo: '',
            sizes: [
              {
                sizeId: 0,
                sizeType: '',
                isSelected: true
              }
            ]
          }

    }
  },
  methods: {
    citySelectInfo: function () {
      this.$http.get("/walking/city")
          .then(response => {
            this.cities = response.data
          })
          .catch(error => {
            console.log(error)
          })
    },

    getDogSizesInfo: function () {
      console.log('olen siin 1')
      console.log()
      this.$http.get("/dog/size")
          .then(result => {
            this.walkingRequest.sizes = result.data
          })
          .catch(error => {
            console.log(error)
          })
    },

    addNewService: function () {
      this.$http.post("/walking/register", this.walkingRequest
      ).then(() => {
        this.$router.push({
          name: ('WalkerActiveServicesRoute')

        })
      }).catch(error => {
        console.log(error)
      })
    },

  },
  beforeMount() {
    this.citySelectInfo()
    this.getDogSizesInfo()
  }
}
</script>

