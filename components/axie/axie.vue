<template>
    <div>
        <v-container class="pa-4 text-center" v-if="axielist.length != 0">
            <v-row
            class="fill-height"
            align="center"
            justify="center"
            >
            
            <template v-for="(item, i) in axielist.axies.results">
                <v-col
                :key="i"
                cols="12"
                md="4"
                sm="4"
                lg="4"
                xl="4"
                >
                <v-hover v-slot="{ hover }" style="cursor:pointer" >
                    <v-card
                    :elevation="hover ? 12 : 2"
                    class="glasseffect"
                    :loading="price.length == 0"
                    :disabled="item.auction == null"
                    @click="()=> {
                        show = true
                        load_axies(item.id,convert_usd_to_weth(item.auction.currentPriceUSD))
                    }"
                    >
                    <v-card-title>
                        <div class="d-flex flex-column text-left caption">
                            <div>
                               <v-chip
                                pill
                                dark
                                x-small
                                :color="coloraxie(item.class)"
                                class="text-truncate"
                                >
                                #{{item.id}}
                                </v-chip>
                            </div>

                            <div class="mt-2">
                                <span :inner-html.prop="item.class|classaxie"></span>
                                {{item.name}}
                            </div>

                            <div>
                                <small>Bread count: {{item.breedCount}}</small>
                            </div>
                        </div>
                    </v-card-title>

                    <v-img
                        :src="item.image"
                        :lazy-src="item.image"
                        height="180px"
                    >
                    </v-img>

                    <v-card-subtitle v-if="price.length != 0 && item.auction != null">
                        <b>Ξ {{convert_usd_to_weth(item.auction.currentPriceUSD)}}</b>
                        <small> <s>${{item.auction.currentPriceUSD}}</s> - ${{item.auction.currentPriceUSD | make_discount}}</small>
                    </v-card-subtitle>
                    <v-card-subtitle v-else>
                        This auction is no longer available. Please try again later.
                    </v-card-subtitle>
                    </v-card>
                </v-hover>
                </v-col>
            </template>
            </v-row>
        </v-container>


  <v-row justify="center">
    <v-dialog
      v-model="show"
      persistent
      max-width="500"
    >


    <v-container 
        v-if="axie.length == 0"
    >
        <v-card height="400" color="#242735">
             <Loads class="pa-15"/>
        </v-card>
    </v-container>
      <v-card
      v-if="axie.length != 0"
      shaped
      color="#242735"
      >

        <v-card-title class="mb-3">
            <v-chip>
                {{axie.axie.id}}
            </v-chip>
        </v-card-title>
          
        <v-card-subtitle class="text-h5 ">
           <span :inner-html.prop="axie.axie.class|classaxie"></span>
            {{axie.axie.name}}
        </v-card-subtitle>
        
        <v-img 
            :lazy-src="axie.axie.image"
            :src="axie.axie.image"
        >
        </v-img>
        

        <v-container>

            <v-card
            outlined
            class="mb-2"
            color="#11131B"
            >
                <v-card-title>Stats</v-card-title>
                <v-card-text>

                    <v-row>
                        <v-col>
                            <h4>Health</h4>
                            <h5>
                                <svg width="16" height="16" viewBox="0 0 16 16" style="fill: var(--color-gray-3);"><g><rect width="16" height="16" rx="4"></rect><path d="M8 12.394s-4.98-1.626-4.98-5.152A2.72 2.72 0 018 5.73a2.72 2.72 0 014.98 1.512c0 3.526-4.98 5.152-4.98 5.152" fill="#3ac279"></path></g></svg>
                                {{axie.axie.stats.hp}}
                            </h5>
                        </v-col>
                        <v-col>
                            <h4>Speed</h4>
                            <h5>
                                <svg width="16" height="16" viewBox="0 0 16 16" style="fill: var(--color-gray-3);"><g><rect width="16" height="16" rx="4"></rect><path d="M9.488 6.85h.96c.16 0 .253.184.157.312L6.56 12.666c-.126.159-.381.047-.349-.154l.697-3.96a.195.195 0 00-.194-.23h-.808a.198.198 0 01-.194-.231l.958-5.009a.197.197 0 01.193-.16h3.586c.135 0 .23.134.183.262L9.305 6.581a.197.197 0 00.183.268" fill="#f7ac0a"></path></g></svg>
                                {{axie.axie.stats.speed}}
                            </h5>
                        </v-col>
                        <v-col>
                            <h4>Skill</h4>
                            <h5>
                                <svg width="16" height="16" viewBox="0 0 16 16" style="fill: var(--color-gray-3);"><g><rect width="16" height="16" rx="4"></rect><path d="M11.374 9.463c-.093.308-.15 3.015-.394 3.205-.246.19-2.677-.59-2.98-.59-.303 0-2.734.78-2.98.59-.245-.19-.302-2.897-.395-3.205-.093-.307-1.54-2.533-1.445-2.84.093-.307 2.488-1.201 2.735-1.39.244-.19 1.782-2.346 2.085-2.346.302 0 1.84 2.155 2.085 2.345.246.19 2.642 1.084 2.735 1.391.095.307-1.352 2.533-1.446 2.84" fill="#6a3ac2"></path></g></svg>
                                {{axie.axie.stats.skill}}
                            </h5>
                        </v-col>
                        <v-col>
                            <h4>Morale</h4>
                            <h5>
                                <svg width="16" height="16" viewBox="0 0 16 16" style="fill: var(--color-gray-3);"><g><rect width="16" height="16" rx="4"></rect><path d="M11.177 9.139c0 .132-.009.261-.023.39-.176 1.959-1.5 3.263-3.336 3.263-1.781 0-3.017-1.232-3.33-3.106a3.407 3.407 0 01-.046-.547l.012-.229c.043-.771.39-1.577.909-2.087l.363-.358.024.509c.012.236.128.458.337.642.107.094.365.195.676.195.161 0 .314-.028.442-.08a.596.596 0 00.397-.532c.023-.325-.107-.486-.272-.69-.184-.226-.413-.507-.534-1.13-.15-.778.287-1.514 1.23-2.073l.482-.285-.146.54a.995.995 0 00-.032.213c-.02.567.444 1.273 1.417 2.157 1.242 1.13 1.419 2.14 1.423 2.98l.007.228z" fill="#c23a3a"></path></g></svg>
                                {{axie.axie.stats.morale}}
                            </h5>
                        </v-col>
                    </v-row>
                    
                </v-card-text>
            </v-card>

            <v-card
            outlined
            color="#11131B"
            >
                <v-card-title>About</v-card-title>
                <v-card-text>
                    <v-row>
                        <v-col>
                            <h4>Class</h4>
                            <h5>{{axie.axie.class}}</h5>
                        </v-col>
                        <v-col>
                            <h4>Breed Count</h4>
                            <h5>{{axie.axie.breedCount}} / 7</h5>
                        </v-col>
                        <v-col>

                        </v-col>
                    </v-row>
                    <h4 class="mt-2">Owner</h4>
                    <div class="d-flex flex-row">
                        <h5>{{axie.axie.ownerProfile.name}}</h5>&nbsp;
                        <small class="ml-2">({{axie.axie.chain}} :&nbsp;{{axie.axie.owner | truncate_address}})</small>
                    </div>
                    
                </v-card-text>
            </v-card>
        </v-container>
        
        <v-card-actions>
            <v-btn
            color="green darken-1"
            text
            @click="resets()"
          >
            Close
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            :href="`https://marketplace.axieinfinity.com/axie/`+ `${this.axie.axie.id}`"
            target="_blank"
          >
            Inspect
          </v-btn>
          <v-btn
            color="green darken-1"
            text
            :disabled="checker != undefined"
            @click="()=> {
                isSuccess(),
                addItem([axie.axie.id,axie.axie.name,axie.axie.image,axie.axie.class,axie.axie.stats,axieprice])
            }"
          >
            Buy
          </v-btn>
        </v-card-actions>
      </v-card>

    </v-dialog>

    <v-dialog
        v-if="axie.length != 0"
        v-model="purchase"
        max-width="400"
    >
      <v-card
      color="#11131B"
      >
            <div v-if="done">

            
                <v-card-title class="mb-3 text-center">
                    <v-icon color="green" class="mr-2">mdi-check-bold</v-icon> Successfully purchased!
                </v-card-title>

                <v-card-text class="text-center">
                    
                    Your transaction is currently on process.<br>

                    {{axie.axie.id}}

                </v-card-text>
            </div>

            <div v-else>

            
                <v-card-title class="mb-3 text-center">
                   Purchase Failed
                </v-card-title>

                <v-card-text class="text-center">
                    
                    Insufficient balance.<br>


                </v-card-text>
            </div>
      </v-card>
    </v-dialog>

  </v-row>
        
    </div>
</template>



<script>
    import { mapState,mapMutations } from 'vuex'
    import _ from 'lodash';
    import Loads from '~/components/axie/axieload.vue'
    export default {
        components:{
            Loads
        },
        props:{
            axielist:Object,
            price:Array
        },
        data: () => ({
            open:false,
            axie:[],
            axieprice:0,
            dialog: false,
            purchase: false,
            done:false,
            show: false
        }),
        computed:{
            checker (){
            if(this.axie.length != 0)
                    return this.$store.getters.check_axie_exist_in_cart(this.axie.axie.id)
            },
            ...mapState(['cart','pointbalance','user']),
        },
        methods:{

            ...mapMutations(['addItem']),
            isSuccess: function (){
                if(this.user.length != 0){
                    this.purchase = true
                    if(this.axieprice <= this.pointbalance){
                        
                        this.done = true;
                    }
                }else{
                    this.$router.push('/login');
                }
            },

          resets : function (){
              this.axie = []
              this.show = false
              
          },

          convert_usd_to_weth (currentprice){
              const calculate = currentprice - (currentprice * 0.20);
              const convert = calculate / this.price[1].price

              let balance = convert.toString();
              let startDigit = balance.substring(0,5);
              return startDigit
            },

            load_axies: _.debounce(async function(id,price){
            this.axie = []
            this.axieprice = 0
            const response = await this.$axios.post('https://axieinfinity.com/graphql-server-v2/graphql',
                {
                    "operationName":"GetAxieDetail",
                    "variables":{
                        "axieId":id
                    },
                    "query":"query GetAxieDetail($axieId: ID!) {\n  axie(axieId: $axieId) {\n    ...AxieDetail\n    __typename\n  }\n}\n\nfragment AxieDetail on Axie {\n  id\n  image\n  class\n  chain\n  name\n  genes\n  owner\n  birthDate\n  bodyShape\n  class\n  sireId\n  sireClass\n  matronId\n  matronClass\n  stage\n  title\n  breedCount\n  level\n  figure {\n    atlas\n    model\n    image\n    __typename\n  }\n  parts {\n    ...AxiePart\n    __typename\n  }\n  stats {\n    ...AxieStats\n    __typename\n  }\n  auction {\n    ...AxieAuction\n    __typename\n  }\n  ownerProfile {\n    name\n    __typename\n  }\n  battleInfo {\n    ...AxieBattleInfo\n    __typename\n  }\n  children {\n    id\n    name\n    class\n    image\n    title\n    stage\n    __typename\n  }\n  __typename\n}\n\nfragment AxieBattleInfo on AxieBattleInfo {\n  banned\n  banUntil\n  level\n  __typename\n}\n\nfragment AxiePart on AxiePart {\n  id\n  name\n  class\n  type\n  specialGenes\n  stage\n  abilities {\n    ...AxieCardAbility\n    __typename\n  }\n  __typename\n}\n\nfragment AxieCardAbility on AxieCardAbility {\n  id\n  name\n  attack\n  defense\n  energy\n  description\n  backgroundUrl\n  effectIconUrl\n  __typename\n}\n\nfragment AxieStats on AxieStats {\n  hp\n  speed\n  skill\n  morale\n  __typename\n}\n\nfragment AxieAuction on Auction {\n  startingPrice\n  endingPrice\n  startingTimestamp\n  endingTimestamp\n  duration\n  timeLeft\n  currentPrice\n  currentPriceUSD\n  suggestedPrice\n  seller\n  listingIndex\n  state\n  __typename\n}\n"
                }
            );
            const store = await response.data;
            this.axieprice = price
             this.axie = store.data;
            
          },1000),
        
          coloraxie : function (axie){

                if(axie == "Beast"){
                    return "#ba8b1e"
                }

                if(axie == "Plant"){
                    return "#6cc000"
                }

                if(axie == "Aquatic"){
                    return "#00b8ce"
                }

                if(axie == "Bird"){
                    return "#503d52"
                }

                if(axie == "Bug"){
                    return "#ff5341"
                }

                if(axie == "Dawn"){
                    return "#129092";
                }

                if(axie == "Reptile"){
                    return "#dc8be4"
                }

                if(axie == "Mech"){
                    return "#c6bdd4"
                }

                if(axie == "Dusk") {
                    
                    return "#beceff"
                }
            
        }   ,
        },
        filters:{

            make_discount : function (currentprice){
              const calculate = currentprice - (currentprice * 0.20);
              return (Math.round(calculate * 100) / 100).toFixed(2);

            },

            truncate_address : function (string){
                var startDigit = string.substring(0,9);
                var lastDigit = string.substr(string.length-5);
                return startDigit+"..."+lastDigit
            },

            classaxie : function (axie){

                if(axie == "Beast"){
                    return '<svg width="16" height="16" viewBox="0 0 16 16" style="fill: #ba8b1e;"><path d="M7.933 4.886a1.91 1.91 0 100-3.82 1.91 1.91 0 000 3.82M12.713 2.635a1.91 1.91 0 100 3.82 1.91 1.91 0 000-3.82M5.064 4.544a1.91 1.91 0 10-3.82 0 1.91 1.91 0 003.82 0M7.916 6.11a4.487 4.487 0 100 8.972 4.487 4.487 0 000-8.973"></path></svg>';
                }

                if(axie == "Plant"){
                    return '<svg width="16" height="16" viewBox="0 0 16 16" style="fill: #6cc000;"><path d="M14.205 4.357c-.796-.634-1.882-.941-2.89-.74C9.764 3.926 8.53 5.31 8 6.757c-.53-1.447-1.764-2.831-3.314-3.14-1.009-.201-2.095.106-2.891.74C.847 5.112.5 6.291.836 7.45c.255.879 1.11 1.204 1.933 1.364.912.178 1.906.33 2.617.997a4.745 4.745 0 011.233 1.946c.073.218.137.44.19.665.049.203.056.415.096.62.036.19.112.106.19.01.392-.485.692-1.08.905-1.696.213.616.513 1.21.905 1.695.078.097.154.18.19-.01.04-.204.048-.416.096-.619.053-.224.117-.447.19-.665a4.745 4.745 0 011.234-1.946c.71-.666 1.703-.82 2.616-.997.823-.16 1.678-.485 1.933-1.364.335-1.16-.011-2.338-.959-3.093"></path></svg>';
                }

                if(axie == "Aquatic"){
                    return '<svg width="16" height="16" viewBox="0 0 16 16" style="fill: #00b8ce;"><path d="M15.036 5.73c-.136-.615-.329-1.207-.989-.985-.3.102-.578.285-.843.47a8.114 8.114 0 00-1.82 1.777c-.646-1.22-1.717-2.15-2.73-2.786-1.575-.99-3.155-1.12-4.78-.239C2.5 4.712 1.326 6.03.94 7.717a2.81 2.81 0 00-.051.304c.012.1.027.202.05.304.387 1.686 1.562 3.005 2.935 3.75 1.625.88 3.205.751 4.78-.24 1.013-.636 2.084-1.565 2.73-2.786a8.108 8.108 0 001.82 1.776c.265.186.542.37.843.471.66.222.853-.369.989-.985.165-.747.21-1.522.189-2.29.02-.768-.024-1.543-.19-2.29"></path></svg>'
                }

                if(axie == "Bird"){
                    return '<svg width="16" height="16" viewBox="0 0 16 16" style="fill: #503d52;"><path d="M9.745 12.248a4.564 4.564 0 01-1.948.837c-.424.076-.857.046-1.278-.036-.386-.074-.86-.148-1.193-.36-.142-.089-.588.97-.645 1.075-.301.554-.783 1-1.357 1.238-.026.011-.056.021-.081.01-.029-.014-.038-.05-.043-.082-.088-.57.1-1.144.433-1.606.085-.117 1.03-1.065.876-1.168-.328-.221-.586-.626-.809-.949-.244-.353-.445-.737-.546-1.157a4.562 4.562 0 01-.014-2.12c.061-.286.246-.661.499-.824.068-.043.487.468.533.519.277.307.504.588.86.818L4.387 6.78c-.04-.102-.08-.212-.058-.32a.579.579 0 01.123-.222c1.52-1.988 3.078-4.134 5.606-4.845.189-.053 2.254-.687 2.36-.452.556 1.222.73 2.593.585 3.923-.128 1.167-.61 2.34-1.004 3.407-.275.743-.846 1.401-1.23 2.096a.58.58 0 01-.154.202c-.09.064-.207.07-.317.076l-1.782.075c.354.234.702.33 1.094.46.065.022.702.2.69.28-.048.297-.317.616-.555.788"></path></svg>';
                }

                if(axie == "Bug"){
                    return '<svg width="16" height="16" viewBox="0 0 16 16" style="fill: #ff5341"><path d="M13.264 3.198c-1.565.33-3.077.994-4.084 2.216A6.19 6.19 0 008 7.704a6.19 6.19 0 00-1.18-2.29C5.813 4.192 4.301 3.527 2.736 3.198c-.357-.075-.75-.181-1.116-.197-.72-.031-.612.948-.616 1.439-.006.666.103 1.328.249 1.979.171.764.311 1.571.851 2.193.356.41.846.692 1.321.972.083.048.169.1.213.183.05.095.033.208.016.313l-.383 2.245c-.029.168-.048.366.081.484a.53.53 0 00.239.105c1.534.367 3.211-.478 4.01-1.774.176-.286.305-.585.399-.893.094.308.223.607.399.893.799 1.296 2.476 2.141 4.01 1.774a.529.529 0 00.238-.105c.13-.118.11-.316.082-.484l-.383-2.245c-.018-.105-.034-.218.016-.313.044-.082.13-.135.212-.183.476-.28.966-.561 1.322-.972.54-.622.68-1.43.851-2.193.146-.65.255-1.313.25-1.98-.005-.49.104-1.47-.617-1.438-.366.016-.759.122-1.116.197z"></path></svg>';
                }

                if(axie == "Dawn"){
                    return '<svg width="16" height="16" viewBox="0 0 16 16" style="fill: #129092;"><path d="M8.05985333,1.06695111 L8.10010932,1.07857965 C8.50590418,1.27947039 9.25287054,4.16489806 10.1120533,6.10864 C12.0905422,6.98668444 14.9276978,7.69868444 15.0453867,8.06277333 L15.0453867,8.06277333 C15.0510756,8.06668444 15.0487644,8.07059556 15.0482311,8.07450667 C15.0487644,8.07824 15.0510756,8.08232889 15.0510756,8.08606222 L15.0510756,8.08606222 C14.9276978,8.45032889 12.0905422,9.16215111 10.1120533,10.0401956 C9.22423111,12.0487289 8.45623111,15.0627733 8.06067556,15.08144 L8.06067556,15.08144 L8.06067556,15.0823289 L8.05765333,15.0821289 C7.66156444,15.0627733 6.89356444,12.0487289 6.00574222,10.0401956 C4.02707556,9.16215111 1.18992,8.45032889 1.07223111,8.08606222 L1.07223111,8.08606222 L1.06672,8.08606222 L1.06956444,8.07450667 C1.06885333,8.07059556 1.06672,8.06668444 1.06672,8.06277333 L1.06672,8.06277333 C1.18992,7.69868444 4.02707556,6.98668444 6.00574222,6.10864 C6.89356444,4.10010667 7.66156444,1.08606222 8.05694222,1.06757333 L8.05694222,1.06757333 L8.05985333,1.06695111 Z M13.0631289,11.3783289 C13.4341511,12.3591289 13.8265067,13.49904 13.6986844,13.62704 C13.5706844,13.7548622 12.4305956,13.3625067 11.4497956,12.9916622 C11.0789511,12.0106844 10.6864178,10.8705956 10.81424,10.7427733 C10.9420622,10.6149511 12.0821511,11.0073067 13.0631289,11.3783289 Z M5.39406222,10.6577067 C5.52170667,10.7855289 5.12935111,11.9256178 4.75832889,12.9064178 C3.77752889,13.27744 2.63761778,13.6697956 2.50961778,13.5419733 C2.38161778,13.4139733 2.77415111,12.2740622 3.14517333,11.2932622 C4.12597333,10.92224 5.26606222,10.5298844 5.39406222,10.6577067 Z M9.58896284,6.44819255 C9.41358578,6.43904672 8.64083556,6.88987852 8.03952,7.12250667 C7.40414222,6.87770667 6.61445333,6.40499556 6.50743111,6.45992889 L6.50743111,6.45992889 L6.50618667,6.45868444 C6.50316444,6.46241778 6.50174222,6.46295111 6.50085333,6.46384 L6.50085333,6.46384 C6.44752,6.57210667 6.92023111,7.36161778 7.16485333,7.99699556 C6.91560889,8.64126222 6.41605333,9.48268444 6.49996444,9.57477333 L6.49996444,9.57477333 L6.52880179,9.58473194 C6.7040557,9.59387852 7.47697185,9.14305778 8.07845333,8.91059556 C8.71383111,9.15521778 9.50316444,9.62810667 9.61036444,9.57317333 L9.61036444,9.57317333 C9.61232,9.57352889 9.61285333,9.57210667 9.61356444,9.57121778 L9.61356444,9.57121778 L9.61676444,9.56926222 C9.67045333,9.46081778 9.19756444,8.67130667 8.95276444,8.03592889 C9.20218667,7.39166222 9.70174222,6.55041778 9.61783111,6.45832889 L9.61783111,6.45832889 L9.61765333,6.45779556 L9.61729778,6.45726222 Z M13.6986844,2.69214222 C13.8265067,2.81996444 13.4341511,3.96005333 13.0631289,4.94067556 C12.0821511,5.31169778 10.9420622,5.70405333 10.81424,5.57623111 C10.6864178,5.44840889 11.0789511,4.30832 11.4497956,3.32752 C12.4305956,2.95649778 13.5706844,2.56414222 13.6986844,2.69214222 Z M4.75832889,3.24245333 C5.12935111,4.22325333 5.52170667,5.36334222 5.39406222,5.49116444 C5.26606222,5.61898667 4.12597333,5.22663111 3.14517333,4.85560889 C2.77415111,3.87498667 2.38161778,2.73489778 2.50961778,2.60689778 C2.63761778,2.47907556 3.77752889,2.87143111 4.75832889,3.24245333 Z"></path></svg>'
                }

                if(axie == "Reptile"){
                    return '<svg width="16" height="16" viewBox="0 0 16 16" style="fill: #dc8be4;"><path d="M13.927 8.056a1.32 1.32 0 00-1.309 1.152c-.135.191-.351.433-.583.476-.189.035-.457-.108-.607-.215a1.779 1.779 0 01-.696-1.186c-.105-.831-.308-2.175.324-2.857.026.001.053.004.08.004a1.47 1.47 0 10-1.263-.72c.058.758-.354 1.554-.954 2a2.022 2.022 0 01-.43.254c-.248.101-.595.1-.84 0a2.018 2.018 0 01-.43-.254c-.6-.446-1.012-1.242-.954-2a1.47 1.47 0 10-1.262.72c.026 0 .052-.003.079-.004.631.682.428 2.026.323 2.857-.057.455-.32.92-.696 1.186-.149.107-.418.25-.607.215-.232-.043-.447-.285-.582-.476a1.32 1.32 0 10-.619 1.294.655.655 0 01.331-.023c.62.122 1.095.604 1.481 1.071.413.499.86.949 1.43 1.262.327.18.672.336 1.029.448.195.06.443.088.647.114.464.06 1.002-.05 1.435-.217.255-.097.502-.214.741-.345.569-.313 1.017-.763 1.43-1.262.386-.467.861-.949 1.48-1.07a.654.654 0 01.331.022 1.321 1.321 0 10.69-2.446"></path></svg>';
                }

                if(axie == "Mech"){
                    return '<svg width="16" height="16" viewBox="0 0 16 16" style="fill: #c6bdd4;"><path d="M9.23004444,9.8361067 C9.52195556,9.87521781 9.84391111,9.86935115 10.1345778,9.83539559 C10.0403556,9.98632892 9.84337778,10.0949511 9.69511111,10.1827734 C9.31342222,10.4090845 8.85333333,10.4679289 8.41653333,10.4544178 C8.07502222,10.4437511 7.73671111,10.3552178 7.43111111,10.2032178 C7.12924444,10.0533511 6.8624,9.88588448 6.65102222,9.61815115 C6.64888889,9.61548448 6.64746667,9.61192892 6.64622222,9.60855115 C6.632,9.59504003 6.61688889,9.58312892 6.60284444,9.56926226 C5.79484444,8.76126226 5.79484444,7.45139559 6.60284444,6.64339559 C6.69173333,6.55468448 6.78737778,6.47735115 6.88675556,6.40819559 C6.59608889,6.36979559 6.27591111,6.37548448 5.98648889,6.40944003 C6.08071111,6.2585067 6.27768889,6.1497067 6.42595556,6.06206226 C6.80782222,5.83557337 7.26791111,5.7769067 7.70453333,5.79024003 C8.04604444,5.8009067 8.38435556,5.88961781 8.68995556,6.04144003 C8.92711111,6.1593067 9.14222222,6.28872892 9.32622222,6.4681067 C9.39626667,6.52197337 9.46435556,6.57939559 9.52871111,6.64339559 C10.3367111,7.45139559 10.3367111,8.76126226 9.52871111,9.56926226 C9.42791111,9.66988448 9.31964444,9.75806226 9.20533333,9.83344003 C9.21333333,9.83432892 9.22204444,9.83521781 9.23004444,9.8361067 M11.1130667,2.74437337 C10.5210667,2.39806226 5.6112,2.40944003 5.008,2.74437337 C4.4048,3.07948448 1.95555556,7.00215115 1.95555556,8.03166226 C1.95555556,9.06117337 4.41617778,12.8983289 5.008,13.3185956 C5.6,13.7388623 10.5098667,13.7280178 11.1130667,13.3185956 C11.7160889,12.9091734 14.1655111,8.86881781 14.1655111,8.03166226 C14.1655111,7.1945067 11.7050667,3.09104003 11.1130667,2.74437337"></path></svg>';
                }

                if(axie == "Dusk") {
                    return '<svg width="16" height="16" viewBox="0 0 16 16" style="fill: #beceff;"><path d="M7.63001266,1.62274177 C8.88782226,1.51583444 10.1636395,1.78756482 11.2673834,2.40169006 C11.8081596,2.7023783 12.3047352,3.08210876 12.7376473,3.52467232 C12.9490102,3.74103673 13.1452758,3.97233538 13.3242613,4.21638278 C13.4370367,4.36991408 13.5407173,4.54329701 13.6156584,4.71831906 C13.6345756,4.76239329 13.6573126,4.81174914 13.6616781,4.86019437 C13.6693177,4.94178634 13.6385773,4.93595834 13.5714577,4.89625511 C13.3972015,4.79371878 13.2387703,4.66732408 13.0688796,4.55823125 C12.8409641,4.41162069 12.5972237,4.28923274 12.3431153,4.19434566 C11.8178001,3.99783287 11.2510128,3.92152253 10.6924107,3.96905713 C8.59278724,4.14735743 7.03521333,5.99592539 7.21310744,8.09819336 C7.39118345,10.2004613 9.23760792,11.759997 11.3370495,11.5816967 C11.8849197,11.5352549 12.4202391,11.3704318 12.8982613,11.0985193 C13.1296328,10.9664787 13.3449974,10.8100334 13.5450828,10.6342829 C13.6867797,10.5097094 13.8115602,10.3864109 13.9381597,10.2479959 C13.9567131,10.2275979 14.0356559,10.1600296 14.0416584,10.2235912 C14.0487524,10.3000837 14.0414765,10.3714766 14.0291076,10.4474227 C13.9960026,10.6508563 13.9345218,10.8337097 13.8461204,11.020752 C13.7593561,11.2048803 13.6558574,11.3806308 13.5419906,11.5487321 C13.3066174,11.8960443 13.0272254,12.2125775 12.7263697,12.5036131 C12.4991819,12.7234379 12.2585337,12.929239 12.0062442,13.1193774 C11.0474713,13.8413206 9.90280093,14.2685857 8.71011004,14.3767679 C5.19260867,14.6752707 2.09947025,12.0625065 1.80097921,8.54057479 C1.50285196,5.01864306 4.11251129,1.92142664 7.63001266,1.62274177 Z M11.5244204,5.52724511 C11.6468362,5.52724511 12.0266347,6.32950539 12.3043896,6.95546875 C12.9326578,7.23320925 13.7011676,7.59581997 13.7011676,7.70673405 C13.7011676,7.818923 12.9159234,8.18827234 12.2838354,8.4671056 C12.005171,9.09980758 11.6362863,9.88622299 11.5244204,9.88622299 C11.4136458,9.88622299 11.0516731,9.11692732 10.7739182,8.48768571 C10.1487423,8.20976308 9.34749118,7.82948625 9.34749118,7.70673405 C9.34749118,7.58543885 10.1312803,7.21208276 10.753364,6.93488863 C11.0302094,6.3120214 11.4030958,5.52724511 11.5244204,5.52724511 Z"></path></svg>'
                }
            }
        },
        
    }
</script>


<style lang="scss" scoped>
.glasseffect{
    backdrop-filter: blur(17px) saturate(132%);
    -webkit-backdrop-filter: blur(17px) saturate(132%);
    background-color: rgba(40, 43, 57, 0.76);
    border-radius: 12px;
    border: 1px solid rgba(255, 255, 255, 0.125);

}
</style>