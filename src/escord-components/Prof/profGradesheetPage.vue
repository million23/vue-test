<template>
  <div class="wrapper">
    <parallax
      class="section page-header header-filter"
      :style="headerStyle"
    ></parallax>
   

    <div class="main main-raised" ref="DownloadComp">
      <div class="section profile-content">
        <div class="container">
       
          <div class="__gradesheet-header md-layout md-gutter md-alignment-top-space-between">

            <div class="__gradesheet-subject md-layout-item md-size-100">
                <h4>
                    <strong>{{getGS.subjectcode}}</strong>
                   {{getGS.subjectdesc}}
 
                
                </h4>
            </div>
            
            <div class="__gradesheet-info md-layout-item md-xsmall-size-100 md-size-70">

                    <h5 class="md-subheading">
                        {{getGS.course_year}}{{getGS.course_section}} | {{getGS.course_short}}
                    </h5>
                    <p class="md-caption __top-md-caption">
                        {{getGS.day}} , {{getGS.time}}
                    </p>
                    <p class="md-caption">
                        {{getGS.semester}} SEMESTER,  SY. {{getGS.sem_startyear}}-{{getGS.sem_endyear}}
                    </p>

            </div>
            
            <div class="__gradesheet-buttons md-layout-item md-xsmall-size-100 md-size-30 md-layout md-gutter md-alignment-center-space-between">

                <div class="md-layout-item md-xsmall-size-25 md-small-size-50 md-large-size-25">
                    <md-button 
                    class="md-esc-accent md-raised md-round md-just-icon"
                    @click="classicModal = true">
                        <md-icon>person_add</md-icon>
                        <md-tooltip md-direction="bottom">Add Student</md-tooltip>
                    </md-button>
                </div>

                <div class="md-layout-item md-xsmall-size-25 md-small-size-50 md-large-size-25">
                    <md-button @click="sendArrayofData"
                    class="md-esc-darkgrey md-raised md-round md-just-icon">
                        <md-icon>save</md-icon>
                        <md-tooltip md-direction="bottom">Save Changes </md-tooltip>
                    </md-button>
                </div>

                <div class="md-layout-item md-xsmall-size-25 md-small-size-50 md-large-size-25">
                    <md-button  @click="archievebtn"
                    class="md-esc-darkgrey md-raised md-round md-just-icon">
                        <md-icon>inventory</md-icon>
                        <md-tooltip md-direction="bottom">Archive Gradesheet</md-tooltip>
                    </md-button>
                </div>

                <div class="md-layout-item md-xsmall-size-25 md-small-size-50 md-large-size-25">
                    <md-button
                    class="md-esc-darkgrey md-raised md-round md-just-icon">
                        <md-icon>download</md-icon>
                        <md-tooltip md-direction="bottom">Download Gradesheet</md-tooltip>
                    </md-button>
                </div>
       
              <!--    <div class="md-layout-item md-xsmall-size-25 md-small-size-50 md-large-size-25">
                    <md-button  @click="refreshGradesheet"
                    class="md-esc-darkgrey md-raised md-round md-just-icon">
                        <md-icon>refresh</md-icon>
                        <md-tooltip md-direction="bottom">Refresh Gradesheet</md-tooltip>
                    </md-button>
                </div> -->

            </div>

          </div>

          <md-divider></md-divider>

           <div v-if='loadingStatus'>
            <md-progress-spinner class="__gradesheet-header md-layout md-gutter md-alignment-top-space-between" md-mode="indeterminate"></md-progress-spinner>
          
          </div>

          <div  v-else  class="__gradesheet-table">

            <md-table 
              v-model="studentList"
              md-sort="studLN"
              md-sort-order="asc">

              <md-table-row class="title">
                <md-table-head class="text-center">Actions</md-table-head>
                <md-table-head class="text-center">Student Number</md-table-head>
                <md-table-head class="text-center">Name</md-table-head>
                <md-table-head class="text-center">Midterm</md-table-head>
                <md-table-head class="text-center">Final Term</md-table-head>
                <md-table-head class="text-center">Remarks</md-table-head>
                
              </md-table-row>

              <md-table-row
              v-for="(_, index) in getrow"
              :key="index">

               <div class="md-layout-item md-xsmall-size-25 md-small-size-50 md-large-size-25">
                    <md-button @click.prevent="sendArrayofData(getrow[index])"
                    class="md-esc-darkgrey md-raised md-round md-just-icon">
                        <md-icon>save</md-icon>
                        <md-tooltip md-direction="bottom">save</md-tooltip>
                    </md-button>
                </div>

                
                <md-table-cell class="text-center">
                  {{getrow[index].student_number}}
                </md-table-cell>



                <md-table-cell>
               {{getrow[index].studentname}}
                </md-table-cell>

               <md-table-cell>
              
                 <md-vuelidated
                  :key="index"
                  :messages="{
                    required: 'Field cannot be blank.',
                    maxLength: 'Input cannot be more than 4 characters.',
                    minValue: 'Field only accepts value from `1.00` to `5.00`.',
                    maxValue: 'Field only accepts value from `1.00` to `5.00`.'
                  }" 
                  field="md-field" 
                  class="has-esc-accent">
                   <md-input 
                    type="number"
                    class="text-center"
                     v-model="getrow[index].midterm"
                   ></md-input> 
                  </md-vuelidated> 
               
                </md-table-cell>

                <md-table-cell>
                
                 <md-vuelidated
                  :key="index"
                  :messages="{
                    required: 'Field cannot be blank.',
                    maxLength: 'Input cannot be more than 4 characters.',
                    minValue: 'Field only accepts value from `1.00` to `5.00`.',
                    maxValue: 'Field only accepts value from `1.00` to `5.00`.'
                  }" 
                  field="md-field" 
                  class="has-esc-accent">
                    <md-input 
                    type="number"
                    class="text-center"
                    v-model="getrow[index].finalterm"></md-input>
                  </md-vuelidated> 
                 
                </md-table-cell>

                <md-table-cell>
                  <md-field class="has-esc-accent">
                  <md-input 
                    v-model="getrow[index].finalgrade"></md-input> <!---edit this var--->
                  </md-field>
                </md-table-cell>                
                
              </md-table-row>

            </md-table>

          </div>

          <div class="__addStudentModal">
            <!-- modal -->
            <modal v-if="classicModal" @close="classicModalHide">

              <!-- modal header -->
              <template slot="header">
                  <h3 class="title text-esc-accent">Add Student</h3>
              </template>

              <!-- modal body -->
              <template slot="body">
                <form @submit.prevent="addValidate" novalidate class="md-layout md-gutter md-alignment-center-left">


                  <!-- addStud modal inputs -->
                  <div class="md-layout-item md-layout md-gutter md-alignment-center-space-between md-size-100">

                    <md-field class="has-esc-accent"
                    :class="getValidationClass('studNum')">
                      <label>Student Number</label>
                      <md-input v-model="addStud.studNum"
                      :disabled="sending"
                      type="number"></md-input>

                      <span class="md-error" v-if="!$v.addStud.studNum.required">Student number is required.</span>
                    </md-field>

                    <div class="md-layout-item md-layout md-gutter md-alignment-center-space-between">

                      <md-field class="has-esc-accent md-layout-item md-size-40"
                      :class="getValidationClass('studLN')">
                        <label>Last Name</label>
                        <md-input v-model="addStud.studLN"
                        :disabled="sending"></md-input>

                        <span class="md-error" v-if="!$v.addStud.studLN.required">Last name is required.</span>
                      </md-field>

                      <md-field class="has-esc-accent md-layout-item md-size-40"
                      :class="getValidationClass('studFN')">
                        <label>First Name</label>
                        <md-input  v-model="addStud.studFN"
                        :disabled="sending"></md-input>

                        <span class="md-error" v-if="!$v.addStud.studFN.required">First name is required.</span>
                      </md-field>
                      
                      <md-field class="has-esc-accent md-layout-item md-size-15">
                        <label>MI</label>
                        <md-input v-model="addStud.studMI"></md-input>
                      </md-field>
                    </div>


                    <div class="md-layout-item md-layout md-gutter md-alignment-center-space-between md-size-100">

                      <md-field class="has-esc-accent md-layout-item md-size-45"
                      :class="getValidationClass('studMG')">
                        <label>Midterm Grade</label>
                        <md-input
                        type="number" v-model="addStud.studMG"
                        :disabled="sending"></md-input>

                        <span class="md-error" v-if="!$v.addStud.studMG.required">Midterm grade is required.</span>
                      </md-field>

                      <md-field class="has-esc-accent md-layout-item md-size-45"
                      :class="getValidationClass('studFG')">
                        <label>Final Term Grade</label>
                        <md-input
                        type="number" v-model="addStud.studFG"
                        :disabled="sending"></md-input>

                        <span class="md-error" v-if="!$v.addStud.studFG.required">Final term grade is required.</span>
                      </md-field>
                    </div>

                    
                  </div>

                  
                  <!-- modal footer -->
                  <div class="md-layout md-gutter md-alignment-center-space-between __modal-buttons">

                    <div class="md-layout-item md-layout md-alignment-center-center">
                        <md-button class="md-esc-darkgrey md-raised md-dense md-round md-layout-item md-size-75" @click="classicModalHide">
                          CANCEL
                        </md-button>
                      </div>

                      <div class="md-layout-item md-layout md-alignment-center-center">
                        <md-button class="md-esc-accent md-raised md-dense md-round md-layout-item md-size-75"
                        type="submit"
                        :disabled="sending"
                        >
                          <md-icon>add</md-icon>
                          ADD 
                        </md-button>
                      </div>
                  </div>
                  
                  <md-snackbar
                    md-position="left"
                    :md-active.sync="studAdded">
                    {{addedStudentInfo}} is added to the gradesheet.
                  </md-snackbar>
                </form>
              </template>

              <template slot="footer"></template>
            </modal>
          </div>

        </div>
      </div>
    </div>
    <vue-headful title="Gradesheet Detail"/>
  </div>
</template>

<script>
// modal import
import { Modal } from "@/components";
import { mapActions, mapGetters, mapMutations} from "vuex";
import axios from "axios"


//validation imports
import { validationMixin } from 'vuelidate'
import { required, maxLength, minValue, maxValue } from 'vuelidate/lib/validators'

export default {
  bodyClass: "profile-page",
  name:'Gradesheetpage',
  components: {
      Modal,
    
 
  },
  mounted() {
          this.$store.dispatch('showgsinfo',{ route: this.$route.params.gradeshid });
         // let studentrow = this.$store.getters.getrow;

        //  studentrow.forEach(student => this.studentList = student);
     // console.log(this.studentList)
          
      },
      created(){
          this.studentGrade = this.$store.getters.getrow;
      },
  data() {
    return {
      /* GRADESHEET TABLE DATA */
      subjCode: "CS 108 ",
      subjUnit: "3",
      subjDesc: "SOFTWARE ENGINEERING 1",
      subjDay: "THU",
      subjTime: "10AM - 1PM",
      subjSem: "1ST SEMESTER",
      subjSY_start: "2021",
      subjSY_end: "2022",
      classProg: "BS COMPUTER SCIENCE",
      classYr: "3",
      classSec: "A",
      profName: "JOEMEN BARRIOS",
      profRank: "MASTER TEACHER III",
    

      studentList: [
    
      ],
      studentGrade: [],

       /*modal default value on load*/
      classicModal: false,

      /*modal--form data*/
      addStud: {
        studNum: null,
        studLN: null,
        studFN: null,
        studMI: null,
        studMG: null,
        studFG: null,
        gradesheetid: this.$route.params.gradeshid,
        finalgrade: 1, //this is for testing only
      },
      studAdded: false,
      sending: false,
      addedStudentInfo: '',
   
     
    };
  },

  mixins: [validationMixin], // for validation

   /* for validation */
   validations: {
       getrow: {
         $each: {
           midterm: {
             required,
             maxLength: maxLength(4),
             minValue: minValue(1.00),
             maxValue: maxValue(5.00)},

           finalterm: {
             required,
             maxLength: maxLength(4),
             minValue: minValue(1.00),
             maxValue: maxValue(5.00)}
         }
       },

       addStud: {
        studNum: {required},
        studLN: {required},
        studFN: {required},
        studMG: {required},
        studFG: {required}
      },
   },
  props: {
    header: {
      type: String,
      default: require("@/assets/img/bg-545454.jpg")
    },
    img: {
      type: String,
      default: require("@/assets/img/escord-team-pictures/stud.png")
    }
  },
  computed: {
    headerStyle() {
      return {
        backgroundImage: `url(${this.header})`
      };
    },
    loadingStatus(){
      return this.$store.getters.loadingStatus
    },
       ...mapGetters({getrow : 'getrow'}),
     ...mapGetters({getGS : 'getGS'}),

  
  },

  methods: {
    /*modal function*/
          ...mapActions({ archgradesheet: "archgradesheet" }),
          ...mapActions({ refreshGS: "showgsinfo" }),

          ...mapActions({ addStudGradesheet: "addStudGradesheet" }),
          ...mapActions({ updateGradesheetData: "updateGradesheetData" }),

 // ...mapMutations(['setspeciGS']),
    sendArrayofData(index){


   // this.updateGradesheetData({route:index.id}, this.index);

  
        axios.put('/api/addgs/'+index.id, index).then((response)=>{
        


            
             }).catch((errors)=>{
  
             this.error =  errors.response.data;
   
             })
    },

    classicModalHide() {
      this.classicModal = false;
    },

    refreshGradesheet(){
        this.refreshGS({ route: this.$route.params.gradeshid })
    },

    /* add student modal validation methods */
    getValidationClass (fieldName) {
      const field = this.$v.addStud[fieldName]

      if (field) {
          return {
            'md-invalid': field.$invalid && field.$dirty
          }
        }
      },
      clearForm () {
        this.$v.$reset()
        this.addStud.studNum = null
        this.addStud.studLN = null
        this.addStud.studFN = null
        this.addStud.studMI = null
        this.addStud.studMG = null
        this.addStud.studFG = null
      },
      addStudent () {
        this.sending = true
     
       this.addStudGradesheet(this.addStud)
       
   
      
        // Instead of this timeout, here you can call your API
      window.setTimeout(() => {
          this.addedStudentInfo = `${this.addStud.studLN}, ${this.addStud.studFN} ${this.addStud.studMI}`
          this.studAdded = true
          this.sending = false
          this.clearForm()
        }, 1500)  

        this.refreshGradesheet();
      },
      addValidate () {
        this.$v.$touch()

        if (!this.$v.$invalid) {
  
          this.addStudent()
            
        }
    },

      archievebtn(){
       
        
      axios.put('api/archievegs/'+ this.$route.params.gradeshid, { 
            status_archieve: '1', }).then((response)=>{
          
       
                  
                //add notification time out here 

       
    
    }).catch((errors)=>{
          
    
          
       
                 })  
 
           },

        

     
  }
};
</script>

<style lang="scss" scoped>
.section {
  padding: 0;
}

.__gradesheet-header, .__gradesheet-table {
    margin: 2rem 0 1.5rem 0 !important;
}

h4 {
    margin: 0 !important;
    width: max-content !important;
}

.md-subheading, .md-caption {
    margin-bottom: 0.25rem !important;
}

.__top-md-caption {
    margin-top: 1rem !important;
}

.md-icon {
    color: red !important;
}

.__gradesheet-info {
    padding-bottom: 1.5rem !important;
}

.md-input {
  width: 0.05rem;
  max-width: 100%;
}

.md-error {
  position: absolute !important;
  top: 3.07em !important;
  left: 0 !important;
  line-height: 0.95em !important;
  text-align: justify;
  font-size: .777rem !important;
}

h3, .h3 {
  font-size: 1.5em !important;
  line-height: 1em !important;
  margin-bottom: 0px !important;
}
.__modal-buttons {
  margin-top: 1em;
}
</style>
