
# Simple Express App with Newrelic and Winston Logging

This is a simple Express application that logs all incoming `GET /` requests using `winston` and reports to New Relic. The app logs request details in different formats and supports both console and file transports.

## Features

- Logs all incoming `GET /` requests.
- Logs errors if an issue occurs during the request.
- Uses `winston` for logging with two file-based transports and one console transport (for non-production environments).
- Integrates with New Relic using the `newrelic` package for monitoring and reporting.

## Setup

### Prerequisites

- [Node.js](https://nodejs.org/) (v12 or higher)
- [Newrelic](https://newrelic.com/) account with the `newrelic` npm package installed and properly configured.
- Basic knowledge of Express.js and Winston logging.

### Installation

1. Clone this repository or download the project files.
   ```bash
   git clone https://github.com/bharatsharma19/Newrelic-Express-Logger-Demo.git
   ```

2. Install the dependencies.
   ```bash
   npm install
   ```

3. Set up New Relic by following their documentation and place your `newrelic.js` configuration file in the root of the project.

4. Ensure you include the New Relic configuration in your app by requiring it at the top:
   ```javascript
   require("newrelic");
   ```

5. Make sure to replace your newrelic license key in the following script to the `package.json` file to include your New Relic app name and license key:
   ```json
   "scripts": {
     "start": "NEW_RELIC_APPLICATION_LOGGING_FORWARDING_ENABLED=true NEW_RELIC_APP_NAME=test NEW_RELIC_LICENSE_KEY=license_key node -r newrelic index.js"
   }
   ```

### Usage

1. Start the Express app.
   ```bash
   npm start
   ```

2. The application will start and listen on `http://localhost:3000`.

3. When you hit the `GET /` endpoint, it will log the request using `winston`:
   - Logs all requests to `combined.log`.
   - Logs errors (randomly generated in this example) to `error.log`.

4. To check the logs:
   - **Console** (non-production mode): View logs directly in the terminal.
   - **Error Logs**: Open `error.log`.
   - **Combined Logs**: Open `combined.log`.

### Example Endpoint

- `GET /` – Responds with a JSON object and logs the request.

### Sample Code

```javascript
require("newrelic");

const express = require("express");
const winston = require("winston");

const logger = winston.createLogger({
  level: "info",
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: "error.log", level: "error" }),
    new winston.transports.File({ filename: "combined.log" }),
  ],
});

if (process.env.NODE_ENV !== "production") {
  logger.add(
    new winston.transports.Console({
      format: winston.format.simple(),
    })
  );
}

const app = express();

app.get("/", (req, res) => {
  logger.info("route hit");
  if (Math.random() < 0.5) {
    logger.error("there was an err");
  }
  res.json({ message: "hi there" });
});

app.listen(3000, () => {
  console.log("listening on port 3000");
});
```

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository.
2. Create a new branch for your feature or bugfix (`git checkout -b feature-name`).
3. Commit your changes (`git commit -m 'Add new feature'`).
4. Push to the branch (`git push origin feature-name`).
5. Open a pull request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
