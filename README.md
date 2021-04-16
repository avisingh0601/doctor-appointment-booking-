#Doctor-appointment-booking
***(reactjs,nodejs,MongoDB,express)***
>In client, that means the front-end is managed by react.js.
>In backend, Express.js is an application framework that works for Node.js while Express.js is running as an internet facing web server. Express.js is very powerful and it can manage HTTP request as an GET and POST requests.


###npm install express mongoose cors dotenv
>Here ‘cors’ that stands for ‘Cross-Origin Resource Sharing’ and it provides an express middleware that can enable cors with different options, so by this package we can easily access some data from a outside of our server.
###npm install -g nodemon
this package makes our development easier. It is a tool that helps to make node.js base applications by automatically restarting your node application when it detect any changed files in the directory.

Afetr this we can create the server files inside the backend folder, to do that create a file named as “server.js”
then make an another file named “.env”. In this you have to enter the MongoDB environment variables. to get that you have to go to mongoDB. Atlas and get the connection string of the database cluster that you created.

###Install dependencies for server
###npm install
###Install dependencies for client
cd client ---> npm install
Connect to your mongodb and add info in .env
Run the client & server with concurrently
npm run dev

Run the Express server only
npm run server

Run the React client only
npm run client
Server runs on http://localhost:5000 and client on http://localhost:3000


Server would be up at http://localhost:5000
>This code will help us to get connect with the backend to our front-end having this code in our server.js file.

```
if(process.env.NODE_ENV === 'production'){
    app.use(express.static('client/build'))
    app.get('*', (req, res) => {
       
       res.sendFile(path.join(__dirname, 'client','build', 'index.html'))
    })
}


const PORT = process.env.PORT || 5000
app.listen(PORT, () =>{
    console.log('Server is running on port', PORT)
})

```
##Api outline for creating the base for our Digital telehealtg clinic
```
> project
   |
   >client
       >node_module
       >src
       >public
    App.js
    GlobalState.js
    #index.css
    index.js
   >.env
   >model
   >middleware
   >node_modules
   >controllers
   >routes
   {}package-lock.json
   {}package.jason
   server.js
   
   
client
  |_src
      |_>api
            |_   -categoryAPI.js
                 -doctoAPI.js
                 -appoinmentAPI.js
                 -userAPI.js
        >components
            |_headers
                 |_>icon
                  -#header.css
                  -Header.js
            |_mainpage
                  |_>auth
                         |_login.js
                          -login.css
                          -register.js
                  >bookappoinment
                             |_bookappoinment.js
                             -bookappoinment.css
                  >categories
                           |_categories.js
                           -categories.css
                  >doctordetail
                            |_doctordetail.js
                            -doctordetail.css
                  >createdoctorprofile
                             |_createdoctorprofile.js
                            -createdoctorprofile.css
                  >doctor
                          |_filter.js
                          -doctor.js
                          -doctor.css
                  Pages.js
controllers
    |_categoryCtrl.js
    -doctorCtrl.js
    -appoinmentCtrl.js
    -userCtrl.js

model
   |_categoryModel.js
    -doctorModel.js
    -appoinmentModel.js
    -userModel.js
    
middleware
    |_auth.js
    -authAdmin.js

routes
   |_categoryRouter.js
    -doctorRouter.js
    -upload.js
    -appoinmentRouter.js
    -userRouter.js
```
In the server.js file we can use app.use() function it is used to mount the specified middleware function  at the path which is being specified.it is mostly used to set up middleware for application.
// Routes
```
app.use('/user', require('./routes/userRouter'))
app.use('/api', require('./routes/categoryRouter'))
app.use('/api', require('./routes/upload'))
app.use('/api', require('./routes/doctorRouter ))
app.use('/api', require('./routes/appoinmentRouter))
```

>// Connect to mongodb
```
const URI = process.env.MONGODB_URL
mongoose.connect(URI, {
    useCreateIndex: true,
    useFindAndModify: false,
    useNewUrlParser: true,
    useUnifiedTopology: true
}, err =>{
    if(err) throw err;
    console.log('Connected to MongoDB')
})
```
In the Router file  we use  (post and get) method  to  server to request data from the specified resource or to send data to a server to create /update a resource.
//userRouter.js file
```
const router = require('express').Router()
const userCtrl = require('../controllers/userCtrl')
const auth = require('../middleware/auth')

router.post('/register', userCtrl.register)

router.post('/login', userCtrl.login)

router.get('/logout', userCtrl.logout)

router.get('/refresh_token', userCtrl.refreshToken)

router.get('/infor', auth,  userCtrl.getUser)

router.patch('/bookappoinment',auth,userCtrl.bookappoinment)

module.exports = router
```

##In the model file at the database schema should be present in such way
//userModel.js file
```
const mongoose = require('mongoose')

const userSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true,
        trim: true
    },
    email: {
        type: String,
        required: true,
        unique: true
    },
    password: {
        type: String,
        required: true
    },
    role: {
        type: Number,
        default: 0
    },
    bookappoinment:{
        "startTime":"10:00",
        "endTime":"10:30",
        "booking id:"",
        "status":"booleen"
    }
}, {
    timestamps: true
})

module.exports = mongoose.model('Users', userSchema)
```

//doctorModel.js
```
database schema for doctor

const mongoose = require('mongoose')


const productSchema = new mongoose.Schema({
    doctor:{
        type: String,
        unique: true,
        trim: true,
        required: true
    },
    title:{
        type: String,
        trim: true,
        required: true
    },
    fees
        type: Number,
        trim: true,
        required: true
    },
    description:{
        type: String,
        required: true
    },
    content:{
        type: String,
        required: true
    },
    images:{
        type: Object,
        required: true
    },
    category:{
        type: String,
        required: true
    },
    checked:{
        type: Boolean,
        default: false
    },
    boooked:{
        type: Number,
        default: 0
    }
}, {
    timestamps: true
})


module.exports = mongoose.model("Doctor",doctorSchema)

```
##in the controllers file
```
const Users = require('../models/userModel')
const Payments = require('../models/paymentModel')
const bcrypt = require('bcrypt')
const jwt = require('jsonwebtoken')

const userCtrl = {
    register: async (req, res) =>{
        try {
            const {name, email, password} = req.body;

            const user = await Users.findOne({email})
            if(user) return res.status(400).json({msg: "The email already exists."})

            if(password.length < 6) 
                return res.status(400).json({msg: "Password is at least 6 characters long."})

            // Password Encryption
            const passwordHash = await bcrypt.hash(password, 10)
            const newUser = new Users({
                name, email, password: passwordHash
            })

            // Save mongodb
            await newUser.save()

    
            const accesstoken = createAccessToken({id: newUser._id})
            const refreshtoken = createRefreshToken({id: newUser._id})

            res.cookie('refreshtoken', refreshtoken, {
                httpOnly: true,
                path: '/user/refresh_token',
                maxAge: 7*24*60*60*1000 // 7d
            })

            res.json({accesstoken})

        } catch (err) {
            return res.status(500).json({msg: err.message})
        }
    },
    login: async (req, res) =>{
        try {
            const {email, password} = req.body;

            const user = await Users.findOne({email})
            if(!user) return res.status(400).json({msg: "User does not exist."})

            const isMatch = await bcrypt.compare(password, user.password)
            if(!isMatch) return res.status(400).json({msg: "Incorrect password."})

            // If login success , create access token and refresh token
            const accesstoken = createAccessToken({id: user._id})
            const refreshtoken = createRefreshToken({id: user._id})

            res.cookie('refreshtoken', refreshtoken, {
                httpOnly: true,
                path: '/user/refresh_token',
                maxAge: 7*24*60*60*1000 // 7d
            })

            res.json({accesstoken})

        } catch (err) {
            return res.status(500).json({msg: err.message})
        }
    },
    logout: async (req, res) =>{
        try {
            res.clearCookie('refreshtoken', {path: '/user/refresh_token'})
            return res.json({msg: "Logged out"})
        } catch (err) {
            return res.status(500).json({msg: err.message})
        }
    },
    refreshToken: (req, res) =>{
        try {
            const rf_token = req.cookies.refreshtoken;
            if(!rf_token) return res.status(400).json({msg: "Please Login or Register"})

            jwt.verify(rf_token, process.env.REFRESH_TOKEN_SECRET, (err, user) =>{
                if(err) return res.status(400).json({msg: "Please Login or Register"})

                const accesstoken = createAccessToken({id: user.id})

                res.json({accesstoken})
            })

        } catch (err) {
            return res.status(500).json({msg: err.message})
        }
        
    },
    getUser: async (req, res) =>{
        try {
            const user = await Users.findById(req.user.id).select('-password')
            if(!user) return res.status(400).json({msg: "User does not exist."})

            res.json(user)
        } catch (err) {
            return res.status(500).json({msg: err.message})
        }
    },

    bookappoinment: async (req, res) =>{
        try {
            const user = await Users.findById(req.user.id)
            if(!user) return res.status(400).json({msg: "User does not exist."})

            await Users.findOneAndUpdate({_id: req.user.id}, {
                bookappoinment: req.body.bookappoinment
            })

            return res.json({msg: "booked"})
        } catch (err) {
            return res.status(500).json({msg: err.message})
        }
    },
   
    }
 }

const createAccessToken = (user) =>{
    return jwt.sign(user, process.env.ACCESS_TOKEN_SECRET, {expiresIn: '7d'})
}
const createRefreshToken = (user) =>{
    return jwt.sign(user, process.env.REFRESH_TOKEN_SECRET, {expiresIn: '7d'})
}

module.exports = userCtrl
```
