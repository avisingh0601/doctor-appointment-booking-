#Doctor-appointment-booking
***(reactjs,nodejs,MongoDB,express)***
>In client, that means the front-end is managed by react.js.
>In backend, Express.js is an application framework that works for Node.js while Express.js is running as an internet facing web server. Express.js is very powerful and it can manage HTTP request as an GET and POST requests.


`npm install express mongoose cors dotenv`
>Here ‘cors’ that stands for ‘Cross-Origin Resource Sharing’ and it provides an express middleware that can enable cors with different options, so by this package we can easily access some data from a outside of our server.
###`npm install -g nodemon`
this package makes our development easier. It is a tool that helps to make node.js base applications by automatically restarting your node application when it detect any changed files in the directory.

After this we can create the server files inside the backend folder, to do that create a file named as “server.js”
then make an another file named “.env”. In this you have to enter the MongoDB environment variables. to get that you have to go to mongoDB. Atlas and get the connection string of the database cluster that you created.
#Prerequisites
##Back-end:
###MongoDB
###Express
###Node.js
```
  "dependencies": {
    "body-parser": "^1.18.2",
    "bcrypt": "^5.0.1",
    "cloudinary": "^1.25.1",
    "concurrently": "^6.0.1",
    "cookie-parser": "^1.4.5",
    "cors": "^2.8.5",
    "dotenv": "^8.2.0",
    "express": "^4.17.1",
    "express-fileupload": "^1.2.1",
    "jsonwebtoken": "^8.5.1",
    "moment": "^2.20.1",
    "moment-timezone": "^0.5.14",
    "mongoose": "^4.13.9",

  },
  "devDependencies": {
    "dotenv": "^4.0.0",
    "eslint": "^4.15.0",
    "eslint-config-prettier": "^2.9.0",
    "eslint-config-standard": "^11.0.0-beta.0",
    "eslint-plugin-import": "^2.8.0",
    "eslint-plugin-node": "^5.2.1",
    "eslint-plugin-prettier": "^2.4.0",
    "eslint-plugin-promise": "^3.6.0",
    "eslint-plugin-standard": "^3.0.1",
    "nodemon": "^1.14.10",
    "now": "^9.2.7",
    "prettier": "^1.10.2"
  }
  ```
#Front-end:

###React.js
  ```
  "dependencies": {
    "axios": "^0.21 .1",
    "jwt-decode": "^2.2.0",
    "moment": "^2.20.1",
    "moment-timezone": "^0.5.14",
    "normalize.css": "^7.0.0",
    "query-string": "^5.0.1",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-paypal-express-checkout": "^1.0.5",
    "react-router-dom": "^5.2.0",
    "react-scripts": "4.0.3"
    "react-datetime": "^2.11.1",
    "react-dom": "^16.2.0",
    "react-modal": "^3.1.11",
    "react-router-dom": "^4.2.2",
    "react-scripts": "1.0.17"
  },
    "devDependencies": {
    "autoprefixer-stylus": "^0.14.0",
    "concurrently": "^3.5.1",
    "stylus": "^0.54.5"
  }
  ```
  
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
    description:{
    type:string
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
##in model appoinment.js file
```
const mongoose = require('mongoose')
const moment = require('moment')


const bookingSchema = new Schema({
  _bookingId: Schema.Types.ObjectId,
  user: { type: Schema.ObjectId, ref: 'User' },
  bookingStart: Date,
  bookingEnd: Date,
  startHour: Number,
  duration: Number,
  recurring: [],
  purpose: { type: String, required: true },
  doctorId: { type: Schema.ObjectId, ref: 'Doctor' }
})

// Validation to ensure a doctor cannot be double-booked
bookingSchema.path('bookingStart').validate(function(value) {
  // Extract the doctor Id from the query object
  let doctorId = this.doctorId
  
  // Convert booking Date objects into a number value
  let newBookingStart = value.getTime()
  let newBookingEnd = this.bookingEnd.getTime()
  
  // Function to check for booking clash
  let clashesWithExisting = (existingBookingStart, existingBookingEnd, newBookingStart, newBookingEnd) => {
    if (newBookingStart >= existingBookingStart && newBookingStart < existingBookingEnd || 
      existingBookingStart >= newBookingStart && existingBookingStart < newBookingEnd) {
      
      throw new Error(
        `Booking could not be saved. There is a clash with an existing booking from ${moment(existingBookingStart).format('HH:mm')} to ${moment(existingBookingEnd).format('HH:mm on LL')}`
      )
    }
    return false
  }
  
  // Locate the doctor document containing the bookings
  return Doctor.findById(doctorId)
    .then(doctor => {
      // Loop through each existing booking and return false if there is a clash
      return doctor.bookings.every(booking => {
        
        // Convert existing booking Date objects into number values
        let existingBookingStart = new Date(booking.bookingStart).getTime()
        let existingBookingEnd = new Date(booking.bookingEnd).getTime()

        // Check whether there is a clash between the new booking and the existing booking
        return !clashesWithExisting(
          existingBookingStart, 
          existingBookingEnd, 
          newBookingStart, 
          newBookingEnd
        )
      })
    })
}, `{REASON}`)


module.exports = mongoose.model("Appoinment",appoinmentSchema)

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
Or we can do it as booking system by this method also.
```
const bookingSchema = mongoose.Schema({
    _id: mongoose.Schema.Types.ObjectId,
    startTime: {type:Number, required:true},
    endTime: {type:Number, required:true},
    clientName: {type:String, required:true}
});

const Booking = mongoose.model('Booking', bookingSchema);
I would store startTime and endTime as timestamps in milliseconds, but that is a matter of choice. You can go all the way down to storing Y / M / D / H / m in separate fields. 

Now, when we add a new booking, we can use some date logic (easier with timestamps) to check if there is a conflict with a preexisting booking :

var newStartTime = someTimestamp;
var newEndTime = someOtherTimestamp;
var newClientName = '';

var conflictingBookings = await Booking.find()
    .where('startTime').lt(newEndTime)
    .where('endTime').gt(newStartTime)
    .exec();

// This selects all preexisting bookings which have both :
// ---> startTime < newEndTime
// ---> endTime > newStartTime
// You can check yourself : I think it handles all possible overlaps !

// Now, conflictingBookings is an array containing all documents in conflict.
// You can tell your barber the problems :

if (conflictingBookings.length === 0) {
    // everything ok, you can book it
    // here you add the new booking to your DB...
} else {

    conflictingBookings.forEach( booking => {
        console.log(`There is already a booking from ${convertToString(booking.startTime)} to ${convertToString(booking.endTime)} with ${booking.clientName} !`);
    });

    // convertToString() being your custom function to convert timestamps to readable dates.
    // we log all conflicting preexisting bookings to the console

}
```
