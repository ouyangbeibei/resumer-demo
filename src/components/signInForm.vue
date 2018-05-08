<template> 
    <div class="wrapper ">

        <div class="modal-login">
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
        </div>              

     
      <!-- <div class="toggle">
      <input type="radio" name="type" v-model="actionType" value="signUp"><span class="button"></span><span　 class="label">注册</span>
    </div>
    <div class="toggle"> 
      <input type="radio" name="type" v-model="actionType" value="login"><span class="button"></span><span　 class="label">登入</span>       
    </div>    -->
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
    .input-inner{
        background-color: #fff;
        background-image: none;
        border-radius: 4px;
        border: 1px solid #dcdfe6;
        box-sizing: border-box;
        color: #606266;
        display: inline-block;
        height: 40px;
        line-height: 40px;
        outline: none;
        padding: 0 15px;
        transition: border-color .2s cubic-bezier(.645,.045,.355,1);
        width: 100%;
        margin: 10px 0;
        &:hover{
            border-color: #c0c4cc;
        }
        &:focus {
            outline: none;
            border-color: #409eff;
        }
    }
    .button-p {
        display: inline-block;
        line-height: 1;
        white-space: nowrap;
        cursor: pointer;
        text-align: center;
        box-sizing: border-box;
        outline: none;
        margin: 0;
        transition: .1s;
        font-weight: 500;
        padding: 10px 18px;
        font-size: 14px;
        border-radius: 4px;
        color: #409eff;
        background: #ecf5ff;
        border-color: #b3d8ff;
        &:hover{
            background: #409eff;
            border-color: #409eff;
            color: #fff;
        }
   }
   .actions {
        text-align: center;
   }
   .row {
       position: relative;
        margin-top: 10px;
   }
      a {
            text-decoration: none;
            color: #444;
        }
        
       
       /* 字体图标定位 */
        .fa {
            position: absolute;
            left: 13px;
            top: 13px;
        }  
        
        
       
        /* 旋转动画 */
         .modal-login, .modal-register {
            transform: translate(-50%,-50%) rotateY(0deg);
            backface-visibility: hidden;
            transition: all 1s;
        }
         .modal-register, .modal-login {
            transform: translate(-50%,-50%) rotateY(180deg);
            backface-visibility: hidden;
            transition: all 1s;
        }
 
</style>