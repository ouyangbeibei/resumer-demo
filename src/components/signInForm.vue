<template> 
    <div class="wrapper ">

        <!-- <div class="modal-login">
       <div class="tabs">
         <a class="login active" href="#">登录</a>
         <a class="register" href="#">注册</a>
       </div>
       <form @submit.prevent="signIn"  v-if="actionType=='login'">
          <div class="row">
            <i class="fa fa-user-o"></i>
            <input class="input-inner" name="username" placeholder="用户名" type="text" required v-model="formData.username">
          </div>
        <div class="row">
            <i class="fa fa-lock"></i>
            <input class="input-inner" name="password" placeholder="密码" type="password" required v-model="formData.password">
        </div>
        <div class="actions">
            <input class="button-p" type="submit" value="登录">
            <span>{{errorMessage}}</span>
        </div>
       </form>
    </div>

    <div class="modal-register"> 
     <div class="tabs">
           <a class="login" href="#">登录</a>
           <a class="register active" href="#">注册</a>
         </div>
     <form @submit.prevent="signUp">
            <div class="row">
                <i class="fa fa-user-o"></i>
                <input class="input-inner" name="username" placeholder="输入用户名" type="text" v-model="formData.username" required>
            </div>
            <div class="row">
                <i class="fa fa-lock"></i>
                <input class="input-inner" name="password" placeholder="输入密码" type="text" v-model="formData.password" required>
            </div>
            <div class=" actions">
                <input class="button-p" type="submit" value="注册">
                <span class="errorMessage">{{errorMessage}}</span>
            </div>
        </form>
        </div>               -->

     <header>
    <a class="login" href="#"><i class="fa fa-user-circle"></i></a>
  </header>
  <main>
    <div class="flip-modal login">
      <div class="modal modal-login">
       <a class="close" href="#"><i class="fa fa-close"></i></a>
       <div class="tabs">
         <a class="login active" href="#">登录</a>
         <a class="register" href="#">注册</a>
       </div>
       <div class="content">
        <div class="errormsg"></div>
          <form @submit.prevent="signIn"  v-if="actionType=='login'">
          <div class="input-field">
            <i class="fa fa-user-o"></i>
             <input class="input-inner" name="username" placeholder="用户名" type="text" required v-model="formData.username">
          </div>
          <div class="input-field">
            <i class="fa fa-lock"></i>
            <input class="input-inner" name="password" placeholder="密码" type="password" required v-model="formData.password">
          </div>   
          <div class="input-field">
            <input class="button-p" type="submit" value="登录">
            <span>{{errorMessage}}</span>
          </div>            
        </form>
       </div>
      </div>


      <div class="modal modal-register">
         <a class="close fa fa-close" href="#"></a>
         <div class="tabs">
           <a class="login" href="#">登录</a>
           <a class="register active" href="#">注册</a>
         </div>
         <div class="content">
          <div class="errormsg"></div>
          <form action="/register" method="post">
            <div class="input-field">
              <i class="fa fa-user-o"></i>
               <input class="input-inner" name="username" placeholder="用户名" type="text" required v-model="formData.username">
            </div>
            <div class="input-field">
              <i class="fa fa-lock"></i>
              <input class="input-inner" name="password" placeholder="密码" type="password" required v-model="formData.password">
            </div>
            <div class="input-field">
              <i class="fa fa-lock"></i>
              <input class="input-inner" name="password2" placeholder="密码" type="password2" required v-model="formData.password">
            </div> 
             <div class="input-field">
                <input class="button-p" type="submit" value="注册">
                <span class="errorMessage">{{errorMessage}}</span>
             </div>
          </form>            
         </div>          
      </div>      
    </div>

  </main>
    </div>
</template>

<script>
    import AV from '../lib/leancloud'
    import getErrorMessage from '../lib/getErrorMessage'
    import getAVUser from '../lib/getAVUser'

    export default{
        name: 'SignInForm',
        data(){
            return {
                actionType: 'signUp',
                formData: {
                    usernam: '',
                    password: ''
                },
                errorMessage:''
            }
        },
        methods: {
            signIn(){
                let {username,password} = this.formData
                AV.User.logIn(username,password).then(()=>{
                    this.$emit('setUser', getAVUser())  //父子通信，//触发自定义setUser的函数。此处的setUser函数就是 setUser函数。
                },(error)=> {
                    this.errorMessage = getErrorMessage(error) //此时不会触发自定义setUser的函数
                });
            },
            signUp(){
                let {username, password} = this.formData
                var user = new AV.User();
                user.setUsername(username);
                user.setPassword(password);
                user.signUp().then(() =>{
                  this.$emit('success', getAVUser())
                }, (error)=> {
                  this.errorMessage = getErrorMessage(error)
                });
            }
        }
    }
</script>

<style lang="scss">

  main {
      background: linear-gradient(to right top, rgba(38,50,105,0.78), rgba(255,152,0,0.45)), url(imgs/bg.jpg) center center no-repeat  ;
      background-size: cover;
    }

    header {
      position: absolute;
      top: 10px;
      width: 100%;
    }

    header .login {
      float: right;
      margin-right: 20px;
      font-size: 20px;
    }

    .flip-modal{
      display: none;
    }
    .show{
      display: block!important;
    }

    .modal {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%,-50%);
      width: 400px;
      padding: 16px;
      background-color: #fff;
      box-shadow: 0 3px 4px 5px rgba(0,0,0,0.2);
    }

    .modal .close {
      position: absolute;
      right: 10px;
      top: 10px;
    }

    .modal .tabs {
      text-align: center;
    }
    .modal .tabs a {
      display: inline-block;
      font-size: 16px;
      margin: 0 10px;
      color: #666;
    }
    .modal .tabs a.active {
      border-bottom: 2px solid #444;
      color: #444;
    }

    .modal .content {
      padding: 20px 40px;
    }
    .modal .errormsg {
      color: #ff9800;
      font-size: 12px;
    }
    .modal .content .input-field {
      position: relative;
      margin-top: 10px;
    }
    .modal .content .input-field .fa{
      position: absolute;
      left: 13px;
      top: 13px;
    }
    .modal .content input[type=text],
    .modal .content input[type=password] {
      width: 100%;
      padding: 10px 10px 10px 28px;
      outline: none;
    }
    .modal .content input[type=submit] {
      padding: 6px 10px;
      border: none;
      background: #fff;
      box-shadow: 0px 0px 2px rgba(0,0,0,0.4);
      outline: none;
    }


    .flip-modal.login .modal-login,
    .flip-modal.register .modal-register {
      transform: translate(-50%,-50%) rotateY(0deg);
      backface-visibility: hidden;
      transition: all 1s;
    }
    .flip-modal.login .modal-register,
    .flip-modal.register .modal-login{
      transform: translate(-50%,-50%) rotateY(180deg);
      backface-visibility: hidden;
      transition: all 1s;
    }

//     .input-inner{
//         background-color: #fff;
//         background-image: none;
//         border-radius: 4px;
//         border: 1px solid #dcdfe6;
//         box-sizing: border-box;
//         color: #606266;
//         display: inline-block;
//         height: 40px;
//         line-height: 40px;
//         outline: none;
//         padding: 0 15px;
//         transition: border-color .2s cubic-bezier(.645,.045,.355,1);
//         width: 100%;
//         margin: 10px 0;
//         &:hover{
//             border-color: #c0c4cc;
//         }
//         &:focus {
//             outline: none;
//             border-color: #409eff;
//         }
//     }
//     .button-p {
//         display: inline-block;
//         line-height: 1;
//         white-space: nowrap;
//         cursor: pointer;
//         text-align: center;
//         box-sizing: border-box;
//         outline: none;
//         margin: 0;
//         transition: .1s;
//         font-weight: 500;
//         padding: 10px 18px;
//         font-size: 14px;
//         border-radius: 4px;
//         color: #409eff;
//         background: #ecf5ff;
//         border-color: #b3d8ff;
//         &:hover{
//             background: #409eff;
//             border-color: #409eff;
//             color: #fff;
//         }
//    }
//    .actions {
//         text-align: center;
//    }
//    .row {
//        position: relative;
//         margin-top: 10px;
//    }
//       a {
//             text-decoration: none;
//             color: #444;
//         }
        
       
//        /* 字体图标定位 */
//         .fa-font {
//             position: absolute;
//             left: 13px;
//             top: 13px;
//         }  
       
     
 
</style>