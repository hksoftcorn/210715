# 업무일지

### ✔ Summary

 

```
09:00~09:50 [강의] DB설계
			팔굽혀 피기(1/3)
10:00~11:05 [미팅] 팀미팅
			팔굽혀 피기(2/3)
11:10~13:00 [코드] FE 로그인 및 회원가입
			팔굽혀 피기(3/3)
			Lunch
13:00~14:00 [미팅] 팀미팅
14:00~16:30 면접 컨설팅

17:00~21:00 [코드] FE 로그인 및 회원가입
			[APS] 알고리즘
			Dinner & 달리기
21:00~22:00 JIRA
22:00~?		[코드] 공통PJT 완료학기
```



## ✨ 오늘 배운 내용

- [프로젝트를 위한 DB 설계하기](./210715_DB설계/RDBMS설계.md)
- [미팅로그](./210715_미팅로그/미팅로그.md)



## 👀 수행한 업무 및 작성한 코드

- 회원가입 및 로그인 유효성 검사

```vue

  setup(props, { emit }) {
    const store = useStore()
    // 마운드 이후 바인딩 될 예정 - 컨텍스트에 노출시켜야함. <return>
    const signinForm = ref(null)

    const state = reactive({
      form: {
        name: '',
        department: '',
        position: '',
        user_id: '',
        password: '',
        passwordConfirm: '',
        align: 'left'
      },
      rules: {
        name: [
          { required: true, message: '필수 입력 항목입니다.', trigger: 'blur' },
          { max: 30, message: '최대 30자까지 입력 가능합니다.', trigger: 'blur' },
        ],
        department: [
          { required: false},
          { max: 30, message: '최대 30자까지 입력 가능합니다.', trigger: 'blur' },
        ],
        position: [
          { required: false},
          { max: 30, message: '최대 30자까지 입력 가능합니다.', trigger: 'blur' },
        ],
        user_id: [
          { required: true, message: '필수 입력 항목입니다.', trigger: 'blur' },
          { max: 16, message: '최대 16자까지 입력 가능합니다.', trigger: 'blur' },
        ],
        password: [
          { required: true, message: '필수 입력 항목입니다.', trigger: 'blur' },
          { min: 9, message: '최소 9자까지 입력 가능합니다.', trigger: 'blur' },
          { max: 16, message: '최대 16자까지 입력 가능합니다.', trigger: 'blur' },
          { pattern: /^(?=.*[A-Za-z])(?=.*\d)(?=.*[$@$!%*#?&])[A-Za-z\d$@$!%*#?&]{0,}$/ , message: '영문, 숫자, 특수문자가 조합되어야 합니다.', trigger: 'blur' },
        ],
        passwordConfirm: [
          { required: true, message: '필수 입력 항목입니다.', trigger: 'blur' },
          { min: 9, message: '최소 9자까지 입력 가능합니다.', trigger: 'blur' },
          { max: 16, message: '최대 16자까지 입력 가능합니다.', trigger: 'blur' },
          { pattern: /^(?=.*[A-Za-z])(?=.*\d)(?=.*[$@$!%*#?&])[A-Za-z\d$@$!%*#?&]{0,}$/ , message: '영문, 숫자, 특수문자가 조합되어야 합니다.', trigger: 'blur' },
          { validator: passwordConfirmValid, trigger: 'blur' },
        ]
      },
      dialogVisible: computed(() => props.open),
      formLabelWidth: '120px',
    })

    // signinForm.value를 확인해봅시다.
    onMounted(() => {
      console.log(signinForm.value)
    })

    // 제대로 동작 x
    var passwordConfirmValid = (rule, value, callback) => {
      if (value !== this.state.form.password) {
        callback(new Error('입력한 비밀번호와 일치하지 않습니다.'));
      } else {
        callback();
      }
    }

    // 아이디 중복 확인 함수입니다.
    const checkId = function () {
      store.dispatch('root/getUserInfo')
      .then(function (result) {
        console.log('')
      })
      console.log('click')
      // axios 부르고 DB에서 아이디 중복 확인한 후
      async
    }


    // 로그인 클릭 함수입니다.
    const clickSignin = function () {
      console.log('click')
      // 로그인 클릭 시 validate 체크 후 그 결과 값에 따라, 로그인 API 호출 또는 경고창 표시
      signinForm.value.validate((valid) => {
        // 만약 유효하다면
        if (valid) {
          // console.log('submit')
          store.dispatch('root/requestSignin', {
            name: state.form.name,
            department: state.form.department,
            position: state.form.position,
            user_id: state.form.user_id,
            password: state.form.password,
            passwordConfirm: state.form.passwordConfirm,
            })
          .then(function (result) {
            console.log('success')
            handleClose()
          })
          .catch(function (err) {
            alert(err)
          })
        } else {
          // 유효하지 않다면
          alert('Validate error!')
        }
      });
    }
    // 닫으면 id와 password를 원래대로 복구합니다.
    // emit을 통해 event를 넘겨줍니다.
    const handleClose = function () {
      state.form.name = ''
      state.form.department = ''
      state.form.position = ''
      state.form.user_id = ''
      state.form.password = ''
      state.form.passwordConfirm = ''
      emit('closeSigninDialog')
    }

    return { signinForm, state, checkId, clickSignin, handleClose }
  }
}
```





## 🐱‍💻 아쉬운 점 & 느낀 점

- 면접 준비를 잘 해보자
- Spring에 대한 이해 부족
- Axios 활용 미숙
- JS에 대한 이해 부족...
