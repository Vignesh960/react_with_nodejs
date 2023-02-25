create api folder
create empty node js folder npm init
add/create index.js
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

and copy paste cors from https://www.npmjs.com/package/cors


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
