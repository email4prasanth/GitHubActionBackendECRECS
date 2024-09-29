# Step 1: Use an official Node.js image as a base
FROM node:16-alpine

# Step 2: Set the working directory inside the container
WORKDIR /usr/src/app

# Step 3: Copy the package.json and package-lock.json (if exists)
COPY package*.json ./

# Step 4: Install dependencies
RUN npm install moment nodemon

# Step 5: Copy the rest of your application code to the container
COPY . .

# Step 6: Expose the port your app runs on (change if needed)
EXPOSE 3000

# Step 7: Command to run the app (make sure the start script is in your package.json)
CMD [ "node", "backend/index.js" ]
