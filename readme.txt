create api folder
create empty node js folder using npm init
add/create index.js 
and add "dev": "nodemon index.js" under scripts in package.json file
npm i nodemon
npm i express
npx prisma
create or add .env file and add PORT=3030
npm install prisma --save-dev
npx prisma init --datasource-provider mysql
then change .env configurations===>DATABASE_URL="mysql://root:root@localhost:3306/blogs"

https://www.prisma.io/docs/getting-started/quickstart
	add model like this prisma\schema.prisma==>
			model user {
  				id String @id @default(cuid())
  				username String
  				password String
  				created_at DateTime @db.Timestamp(6) @default(now())
				}

after adding models/tables then migrate using command:
		npx prisma migrate dev --name init


npm install cors

and copy paste cors from https://www.npmjs.com/package/cors into api/index.js


var express = require('express')
var cors = require('cors')
var app = express()
 
var whitelist = ['http://example1.com', 'http://example2.com']
var corsOptions = {
  origin: function (origin, callback) {
    if (whitelist.indexOf(origin) !== -1) {
      callback(null, true)
    } else {
      callback(new Error('Not allowed by CORS'))
    }
  }
}
app.use(cors(corsOptions))
app.use(express.json())


================X==================
Creating user:-username,password
	1.check username is already existed or 
	2.if not then create if existed prevent user.
for that make sure to create routes for backend api
	routes\auth
		|---index.js
			in this file we need to configures the routes for the backend
			
			const express = require("express");
			//import modules like this
			const preventuser = require("../../middlewares/auth/preventuser");
			const signupvalidation = require("../../vaidators/auth/signup");
			const router = express.Router();
			router.post("/signup",signupvalidation,preventuser,hashpassword,createUser, (req, res) => {
  			try {
   			 res.status(200).send({ msg: "Signup page" });
  			    } catch (error) {}
			});
			//export module like this
			module.exports = router;
create middlewares
	api\middlewares\auth
			|----checkusername.js(develop the func for checking the existing username existed or not)
						//initialize PrismaClient
						const { PrismaClient } = require("@prisma/client");
						const prisma = new PrismaClient();
						const checkUser = (username) =>
						new Promise(async (resolve, reject) => {
							const result = await prisma.user.findFirst({
							where: {
								username: username,
							},
							});
							if (result) {
								resolve({ message: "user exists",password :result.password});
							} else {
								reject({ message: "user doesnt exists", title: "dublicate request" });
							}
						});


						const checkuser = async (req, res, next) => {
						try {
							const { username } = req.body;
							const result = await checkUser(username);
							
							req.hashpassword=result.password
							next();
						} catch (error) {
							console.log(error)
							res.status(400).send(error || { message: "unkown error" });
						}
						};

						module.exports = checkuser
			|--createuser.js(develop the func for creating username)
					const { PrismaClient } = require("@prisma/client");
					const prisma = new PrismaClient();
					const createUser=async (req,res)=>{
						try {
						const {username,password}=req.body
						const result=await prisma.user.create({username:username,password:password});
						next();
						} catch (error)
							{
								console.log(error)
								res.status(404).send(error || {msg:"Unknown Error"})
							}
						}

configure the entry point for middleware in entry point file:-index.js belows
		
		const auth=require("./routes/auth")
		//make use of the auth
		app.use("/auth",auth)
		app.get("*",async (req,res)=>{
			try {
    				res.status(404).send({ msg: "Page not Found" });
  			} catch (error) {}
			
		})


