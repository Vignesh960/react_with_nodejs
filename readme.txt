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
			inthis file we need to configures the routes for the backend

create middlewares
	api\middlewares\auth
			|----checkusername.js(develop the func for checking the existing username existed or not)

configure the entry point for middleware in entry point file:-index.js belows
		
		const auth=require("./routes/auth")
		//make use of the auth
		app.use("/auth",auth)
		app.get("*",async (req,res)=>{
			try {
    				res.status(404).send({ msg: "Page not Found" });
  			} catch (error) {}
			
		})


