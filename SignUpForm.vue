<template>
  <form @submit.prevent="handleSubmit">
    <label>Email:</label>
    <input type="email" required v-model="email">

    <label>Password:</label>
    <input type="password" required v-model="password" @input="validatePassword">
    <span v-if="passwordError" class="error">{{ passwordError }}</span><br>

    <label>Role:</label>
    <select v-model="role">
        <option value="developer">web developer</option>
        <option value="designer">web designer</option>
    </select>

    <label>Skills:</label>
    <input type="text" v-model="tempSkills" @keyup.alt="addSkills">
    <div v-for="skill in skills" :key="skill" class="pill">
        <span @click="deleteSkill(skill)">{{ skill }}</span>
        </div>

    <div class="terms">
        <input type="checkbox" v-model="terms" required>
        <label>Accept Terms & Conditions</label>
    </div>

    <div class="submit">
        <button   @click="start">Submit</button>
         <p v-if="submitted" class="success-message">Form submitted successfully!</p>

    </div>
  </form>
  <p>Email: {{ email }}</p>
  
  <p>Role: {{ role }}</p>
  <p>Terms Accepted: {{ terms }}</p>
  
  
  <!-- <p v-if="reactionTime > 0">Reaction time: {{ reactionTime }} ms</p> -->
   
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      role: 'designer',
      terms: false,
      tempSkills: '',
      skills: [],
      passwordError: '',
      submitted: false,
      timer: null,
      reactionTime: 0
    };
  },
  methods: {
    addSkills(e) {
      if (e.key === ',' && this.tempSkills) {
        if (!this.skills.includes(this.tempSkills)) {
          this.skills.push(this.tempSkills);
        }
        this.tempSkills = '';
      }
    },
    deleteSkill(skill) {
      this.skills = this.skills.filter((item) => {
        return skill !== item;
      });
    },
    validatePassword(){
        this.passwordError = this.password.length > 5 ? '' : 'Password must be at least 6 characters long';
    },
    handleSubmit() {
     
      this.passwordError = this.password.length > 5 ? '' : 'Password must be at least 6 characters long';
      if (!this.passwordError) {
        console.log('Email:', this.email);
        console.log('Password:', this.password);
        console.log('Role:', this.role);
        console.log('Skills:', this.skills);
        console.log('Terms accepted:', this.terms);
        this.submitted = true;
        this.stopTimer(); 
      } else {
        this.startTimer(); 
      }
    },
    startTimer() {
      this.timer = setInterval(() => {
        this.reactionTime += 10;
      }, 10);
    },
    stopTimer() {
      clearInterval(this.timer);
      this.$emit('end', this.reactionTime);
    }
    
  }
}
</script>

<style>
form{
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}
label{
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}
input,select{
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
}
input[type="checkbox"]{
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
}
.pill{
    display: inline-block;
    margin: 20px 10px 0 0 ;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
}
button{
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}
.submit{
    text-align: center;
}
.error{
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
.success-message {
  color: #00cc00; 
  margin-top: 10px;
  font-size: 1em;
  font-weight: bold;
}


</style>

