
---

# Currency Conversion Application

This is a Symfony(7.1) application for fetching, storing, and converting currency exchange rates. It pulls exchange rates from two sources: the European Central Bank (ECB) and the Central Bank of Russia (CBR). The application can convert currencies directly or indirectly via available base currencies (EUR or RUB) and provides a REST API endpoint for currency conversion.

## Features

- Fetch daily exchange rates from the ECB and CBR.
- Store exchange rates in a database using Doctrine ORM.
- Support for conversion between any two currencies, including indirect conversions via EUR or RUB as base currencies.
- REST API endpoint to handle currency conversion requests.
- Unit and integration tests for command and service functionality.

## Prerequisites

- PHP 8.1 or higher
- Composer
- Symfony CLI
- MySQL or another supported database

## Installation

1. **Clone the Repository**

   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Install Dependencies**

   ```bash
   composer install
   ```

3. **Configure Environment Variables**

   Copy `.env.example` to `.env` and update the necessary variables:

   ```bash
   cp .env.example .env
   ```

   Update the following environment variables:

   ```env
   DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/currency_conversion_db
   ECB_URL=https://www.ecb.europa.eu/stats/eurofxref/eurofxref-daily.xml
   CBR_URL=https://www.cbr.ru/scripts/XML_daily.asp
   ```

4. **Set Up the Database**

   ```bash
   php bin/console doctrine:database:create
   php bin/console doctrine:migrations:migrate
   ```

5. **Run the Application**

   You can use the Symfony CLI to run the development server:

   ```bash
   symfony serve
   ```

   The application will be available at `http://127.0.0.1:8000`.

## Usage

### 1. Fetching Exchange Rates

The application provides a Symfony console command to fetch and store daily exchange rates from the ECB or CBR:

```bash
php bin/console app:fetch-exchange-rates [ECB|CBR]
```

- Replace `[ECB|CBR]` with either `ECB` or `CBR` to specify the data source (defaults to `ECB` if omitted).

### 2. Converting Currencies

The application provides an API endpoint for currency conversion. You can use `curl` or any REST client to interact with it.

- **Endpoint**: `/api/convert`
- **Method**: `POST`
- **Request Body**:

  ```json
  {
    "from": "USD",
    "to": "CZK",
    "amount": 100
  }
  ```

- **Example Request**:

  ```bash
  curl --location 'http://127.0.0.1:8000/api/convert' \
  --header 'Content-Type: application/json' \
  --data '{
      "from": "USD",
      "to": "CZK",
      "amount": 100
  }'
  ```

- **Response**:

  ```json
  {
    "from": "USD",
    "to": "CZK",
    "original_amount": 100,
    "converted_amount": 2500.00
  }
  ```

### 3. Testing

The application includes unit and integration tests to ensure functionality.

- **Run All Tests**:

  ```bash
  php bin/phpunit
  ```

- **Run Specific Tests**:

  ```bash
  php bin/phpunit tests/Service/CurrencyConverterTest.php
  php bin/phpunit tests/Command/FetchExchangeRatesCommandTest.php
  ```

## Project Structure

- **`src/Command/FetchExchangeRatesCommand.php`**: Console command to fetch exchange rates from ECB or CBR.
- **`src/Service/CurrencyConverter.php`**: Service to handle currency conversion, supporting both direct and indirect conversions.
- **`src/Controller/Api/CurrencyController.php`**: REST API controller for handling currency conversion requests.
- **`tests/`**: Contains unit and integration tests.


## License

This project is open-source and available under the [MIT License](LICENSE).

---