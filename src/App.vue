<template>
  <form 
    @submit="checkForm"
  >
    <div>
      <p v-if="errors.length">
        <b>به خطاهای زیر توجه فرمایید</b>
        <ul>
          <li 
            v-for="error in errors" 
            :key="error.index"
          >
            {{ error }}
          </li>
        </ul>
      </p>
      <div class="notification">
        <!--USER NAME-->
        <div class="field form-group">
          <label class="label">نام کاربری</label>
          <div class="control">
            <input
              v-model="name"
              class="input"
              type="text"
              placeholder="نام کاربری"
            >
          </div>
        </div>
        <!--NATIONAL CODE-->
        <div class="field">
          <label class="label">کد ملی</label>
          <div class="control has-icons-left has-icons-right">
            <input
              v-model="nationalCode"
              class="input is-success"
              type="text"
              placeholder="کد ملی"
            >
            <span class="icon is-small is-left">
              <i class="fas fa-user" />
            </span>
            <span class="icon is-small is-right">
              <i class="fas fa-check" />
            </span>
          </div>
        </div>
        <!----telephon------>
        <div class="field">
          <label class="label">موبایل</label>
          <div class="control has-icons-left has-icons-right">
            <input
              id="phone"
              v-model="phone"
              class="input is-success"
              type="tel"
              placeholder="موبایل"
            >
            <span class="icon is-small is-left">
              <i class="fas fa-user" />
            </span>
            <span class="icon is-small is-right">
              <i class="fas fa-check" />
            </span>
          </div>
          <!--<p class="help is-danger" id="phone-messsage"></p>-->
        </div>
        <!----pass------>
        <div class="field">
          <label class="label">رمز عبور</label>
          <div class="control has-icons-left has-icons-right">
            <input
              id="pwd"
              v-model="pwd"
              class="input is-success"
              type="password"
              placeholder="رمز عبور"
            >
            <span class="icon is-small is-left">
              <i class="fas fa-user" />
            </span>
            <span class="icon is-small is-right">
              <i class="fas fa-check" />
            </span>
          </div>
          <!--<p class="help is-danger" id="password-messsage"></p>-->
        </div>
        <!----Confirm pass------>
        <div class="field">
          <label class="label">تکرار رمز عبور</label>
          <div class="control has-icons-left has-icons-right">
            <input
              id="confirm-pwd"
              v-model="confirmPwd"
              class="input is-success"
              type="password"
              placeholder="تکرار رمز عبور"
            >
            <span class="icon is-small is-left">
              <i class="fas fa-user" />
            </span>
            <span class="icon is-small is-right">
              <i class="fas fa-check" />
            </span>
          </div>
          <!--<p class="help is-danger" id="confirm-messsage"></p>-->
        </div>
        <div class="field is-grouped">
          <div class="control">
            <button class="button is-link">
              ثبت نام
            </button>
          </div>
          <div class="control">
            <button
              class="button is-link is-light"
              v-on="formSubmit"
            >
              لغو
            </button>
          </div>
        </div>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      errors: [],
      name: null,
      confirmPwd: null,
      pwd : null,
      phone : null,
      nationalCode: null,
    }
  } ,
  methods: {
    checkForm: function (e) {
      this.errors = [];

//****************USERNAME*****************//
      if (!this.name) {
        this.errors.push("پر کردن نام کاربری الزامی است");
      } else if (!this.validUser(this.name)) {
        this.errors.push('نام کاربری معتبر نیست');
      }

//****************TELEPHONE*****************//
      if (!this.phone) {
        this.errors.push('پر کردن شماره تلفن همراه الزامی است');
      } else if (!this.validPhone(this.phone)) {
        this.errors.push('تلفن همراه معتبر نیست');
      }

//****************NATIONAL CODE*****************//
      if (!this.nationalCode) {
        this.errors.push('پر کردن کد ملی الزامی است');
      } else if (!this.validNationalCode(this.nationalCode)) {
        this.errors.push('کد ملی معتبر نیست');
      }

//****************PASSWORD*****************//
      if (!this.pwd) {
        this.errors.push('رمز عبور را انتخاب نمایید');
      } else if (!this.validPassword(this.pwd)) {
        this.errors.push(' رمز عبور معتبر نیست');
      }

//****************CONFIRMPASSWORD*****************//
      if (!this.confirmPwd) {
        this.errors.push('رمز عبور خود را مجددا وارد نمایید');
      } else  if (!this.validConfirmpwd(this.confirmPwd)) {
        this.errors.push('رمز عبور مجدد معتبر نیست');
      }

      if (!this.errors.length) {
        return true;
      }

      e.preventDefault();
    },
    //delete data form
    formSubmit: function(event) {
      // this.submitted.name = this.name;
      // this.submitted.confirmPwd = this.confirmPwd;
      // this.submitted.pwd = this.pwd;
      // this.submitted.phone = this.phone;
      // this.submitted.nationalCode = this.nationalCode;
      
      this.name ='';
      this.confirmPwd ='';
      this.pwd ='';
      this.phone ='';
      this.nationalCode ='';
      
      event.target.reset();
    },

    validUser: function (name) {
      var re = /^(?=.{3,20}$)(?![_.])(?![0-9])[a-zA-Z0-9._]+(?<![_.])$/;
      return re.test(name);
    },

    validPhone : function (phone) {
      //console.log(phone);
      var ph =  /^\(?([0]{1})\)?([9]{1})?([0-9]{9})$/;
      return ph.test(phone);
    },

    validPassword : function (pwd) {
      var pass =  /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,20}$/;
      //console.log(pass.test(pwd));
      return pass.test(pwd);
    },

    validConfirmpwd : function () {
      //console.log(confirmPwd);
      //console.log(this.pwd === this.confirmPwd) ; 
      return (this.pwd == this.confirmPwd);
    },

    validNationalCode : function (nationalCode) {
      var code = /^\(?([0-9]{1})\)?([0-9]{9})$/;
      return (code.test(nationalCode));
    }
    
  }
}
  

</script>