<template>
  <div>
    <div class="head">
      <div class="menu-step-active" :class="{'menu-step-active':page==1}">1</div>
      <div class="menu-step-down" :class="{'menu-step-active':page==2,'menu-step-active':page>1}">2</div>
      <div class="menu-step-down" :class="{'menu-step-active':page==3}">3</div>
    </div>
    <div>
      <p><span> </span>Back to cart</p>
    </div>
     <form>
        <div class="content">    
          <div class="cOne">
            <div v-if="page==1">   
                <div class="title">
                  <h1>Delivery details</h1>
                </div>
                <div class="row">
                  <div>
                    <input type="email" class="form-control" name="email" id="email" placeholder="Email" required>
                  </div>
                  <div>
                    <input type="text" class="form-control" name="dropName" id="dropName" placeholder="Dropshipper name">
                  </div>
                </div>
                <div class="row">
                  <div>
                    <input type="text" class="form-control" name="phone" id="phone" placeholder="Phone Number">
                  </div>
                  <div>
                    <input type="text" class="form-control" name="dropNumber" id="dropNumber" placeholder="Dropshipper Phone Number">
                  </div>
                </div>
                <div class="row">
                  <div>
                    <textarea type="text" class="form-control" name="address" id="address" placeholder="Delivery Address" style="height: 125px;"></textarea>
                  </div>
                </div>   
            
            </div>
            <div v-if="page==2">
              <div>
                <div class="title">   
                  <h1>Shipment</h1>
                </div>
                <div class="row-3">
                  <div v-for="item in shipment" :key="item.id">
                    <div v-if="id[0]==item.id">
                        <div class="boxactive" @click="choiceShipment(item)">
                          <p>{{item.title}} <span> <fa icon="check" class="check"/></span></p>          
                          <p>{{item.price.toString().replace(/(\d)(?=(\d{3})+(?:\,\d+)?$)/g, "$1\,")}}</p>                  
                      </div>
                    </div>
                    <div v-else>
                      <div class="box" @click="choiceShipment(item)">
                        <p>{{item.title}}</p>
                        <p>{{item.price.toString().replace(/(\d)(?=(\d{3})+(?:\,\d+)?$)/g, "$1\,")}}</p>
                      </div>
                    </div>       
                  </div>     
                </div>
                <div class="title" style="margin-top:30px">
                  <h1>Payment</h1>
                </div>
                <div class="row-3">
                <div v-for="item in payment" :key="item.id">
                    <div v-if="idPayment[0]==item.id">
                        <div class="boxactive" @click="choicePayment(item)">
                          <p>{{item.title}} <span> <fa icon="check" class="check"/></span></p>
                          <p>{{item.desc}}</p>                  
                        </div>     
                    </div>
                    <div v-else>
                        <div class="box" @click="choicePayment(item)">
                          <p>{{item.title}}</p>
                          <p>{{item.desc}}</p>
                        </div> 
                    </div>           
                  </div>
                </div>
              </div>
            </div>
            <div class="step-foot">
              <div>
                <a class="link_text" href="#" @click.prevent="prev()" :disabled="disable">Prev</a>
              </div>
              <div>
                <a class="link_text" href="#" @click.prevent="next()" :disabled="disable">Next</a>
              </div>
            </div>    
          </div>
          <div class="cTwo">
              <div class="title">
                <h2>Summary</h2>
                <p class="items">10 item purchased</p>
              </div>
              <div class="ship">
                <p class="items">Delivery estimation</p>
                <p class="textGreen">today by {{titleShipment[0]}}</p>
            </div>
              <div class="ship">
                <div class="parent">
                  <p class="items">Cost of goods</p>
                  <p class="detItems">500,000</p>
                  <p class="items">{{titleShipment[0]}} shipment</p>
                  <p class="detItems">{{price[0]}}</p>
                  <h2 class="items">  Total : </h2>
                  <h2 class="detItems">{{totalPrice.toString().replace(/(\d)(?=(\d{3})+(?:\,\d+)?$)/g, "$1\,")}}</h2>  
                  <div>
                
                </div>  
              </div>
              </div>      
              <div class="ship">
                <button class="btn-payment"> Continue to Payment </button>
              </div>
          </div> 
        </div>  
    </form>  
  </div>  
</template>
<script>
import { useVuelidate } from '@vuelidate/core'
import { required, email } from '@vuelidate/validators'
  export default {
     setup () {
      return { v$: useVuelidate() }
    },
    data() {
      return {
        email: '',
        page:localStorage.getItem("stepper"),
        disable:false,
        isActive:false,
        titleShipment:[],
        price:[],
        id:[],
        idPayment:[],
        titlePayment:[],
        descPayment:[],
        total:500000,
        shipment:[
          {
            "id":1,
            "title":"GO-SEND",
            "price":15000       
          },
          {
            "id":2,
            "title":"JNE",
            "price":9000       
          },
          {
            "id":3,
            "title":"Personal Courier",
            "price":29000      
          },
        ],
        payment:[
          {
            "id":1,
            "title":"e-wallet",
            "desc":"1,500,000 left"      
          },
          {
            "id":2,
            "title":"Bank Transfer",
            "desc":""       
          },
          {
            "id":3,
            "title":"Virtual Account",
            "desc":""     
          },
        ]
      }
    },
    validations () {
      return { 
       email: { required, email } // Matches this.contact.email    
      }
    },
    computed:{
      totalPrice(){
        let total = this.total
        let shipPrice  = this.price
        if(shipPrice[0]==undefined) return total
        return total + shipPrice[0]
      }
    },
    methods: {
      prev() {      
        if(this.page == 1){
          this.disable = true
        }else{
          this.page--
          localStorage.setItem("stepper", this.page)
        }
      },
      next() {   
        if(this.page == 3){
          this.disable = true
        }else{
          this.page++
          localStorage.setItem("stepper", this.page)
        }
      },
      choiceShipment(item) {      
        let items = JSON.stringify(item)
        this.titleShipment.push(JSON.parse(items).title)
        this.id.push(JSON.parse(items).id)
        this.price.push(JSON.parse(items).price)
        let z = this.id.reverse()
        let y = this.price.reverse()
        let x = this.titleShipment.reverse()   
      },
      choicePayment(item){
        let items = JSON.stringify(item)
        this.titlePayment.push(JSON.parse(items).title)
        this.idPayment.push(JSON.parse(items).id)
        this.descPayment.push(JSON.parse(items).desc)
        let z = this.idPayment.reverse()
        let y = this.descPayment.reverse()
        let x = this.titlePayment.reverse()
      }
    },
  }
</script>
<style scoped>
.head{
  width: 500px;
  height: 70px;
  background-color: #fffae6;
  border-radius: 30px;
  margin:-60px auto 0px;
  display: flex;
  justify-content: space-between;
}
.menu-step-down{
  background-color:#ff880036 ;
  height: 40px;
  width:40px;
  text-align: center;
  color: #FF8A00;
  padding: 5px;
  border-radius: 100%;
  margin: 20px;
}
.menu-step-active{
  background-color:#FF8A00 ;
  height: 40px;
  width:40px;
  text-align: center;
  color: #ffffff;
  padding: 5px;
  border-radius: 100%;
  margin: 20px;
}
.title,h1,h2{
  color:#FF8A00;
  font-weight: bold;
}
.content{
  display: flex;
}
.cOne{
  width: 75%;
}
.cTwo{
  width:25%;
  padding: 0px 10px;
  border-left: 1px solid #ff8a0033;
  margin:0 10px ;
}
.form-control {
    height: 45px;
    width: 100%;
    background: #fff;
    color: #000;
    font-size: 14px;
    border-radius: 2px;
    -webkit-box-shadow: none !important;
    box-shadow: none !important;
    border: 1px solid rgba(0, 0, 0, 0.1);
}
.row{
  display: grid; 
  grid-template-columns: 2fr 1fr; 
  gap: 10px;
  margin: 10px 0;
}
p.items{
  color: #82817f;
  font-size: 12px;
}
button.btn-payment {
    height: 60px;
    width: 100%;
    background-color: #FF8A00;
    border-color:#FF8A00;
    border-radius: 5px;
}
.ship{
 margin-top: 30px;
 font-size: 14px;
}
.step-foot{
  display: flex;
  justify-content:space-between;
  margin-top:30px ;
}
.row-3 {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
}
.box {
    width: 200px;
    cursor: pointer;
    border: 1px solid black;
    padding-left:10px ;
    height: 60px;
    font-size: 14px;
}
.boxactive {
    width: 200px;
    cursor: pointer;
    border: 3px solid #1BD97B;
    background-color:#1bd97a1d ;
    padding-left:10px ;
    height: 60px;
    font-size: 14px;

}
.box > p{
  font-weight: bold;
}

a.link_text {
    text-decoration: none;
    color: black;
}
.check{
   color:#1BD97B;
   text-align: right;
}
.textGreen{
  color:#1BD97B;
  font-weight: bold;
}
.parent {
  display: grid;
  grid-template-columns:1fr 1fr;
  grid-template-rows: repeat(2, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
}
.detItems {
  text-align: end;
}

</style>
