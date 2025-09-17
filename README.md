# Places Search Automation with n8n

This project is an n8n workflow that automates the process of searching for places using Google Places API based on user input from Telegram, and saves the results to Google Sheets.

## Workflow Overview

The automation workflow performs the following steps:

1. **Telegram Trigger**: Listens for messages from users on Telegram
2. **Query Building**: Constructs a search query based on the Telegram message
3. **Google Places API Integration**: Searches for places using the Google Places API
4. **Pagination Handling**: Manages pagination for results with more than one page
5. **Data Processing**: Extracts relevant information from the API response
6. **Data Storage**: Saves the extracted data to Google Sheets

## Prerequisites

- n8n instance (local or cloud)
- Google Places API key
- Telegram Bot Token
- Google Sheets with appropriate permissions

## Setup Instructions

### 1. Import the Workflow

1. Open your n8n instance
2. Click on "Add Workflow"
3. Select "Import from File"
4. Choose the `Places Otomasyon.json` file

### 2. Configure Credentials

#### Telegram Credentials
1. Create a Telegram bot using BotFather
2. Obtain the bot token
3. In n8n, go to Settings > Credentials
4. Add new Telegram credentials with your bot token

#### Google Sheets Credentials
1. Create a Google Cloud project
2. Enable Google Sheets API
3. Create OAuth credentials
4. In n8n, add new Google Sheets credentials using OAuth

#### Google Places API Key
1. In your Google Cloud project, create an API key
2. Enable Places API for this key
3. Add the API key as an environment variable in n8n:
   ```
   GOOGLE_PLACES_API_KEY=your_api_key_here
   ```

### 3. Update Node Configurations

1. **Telegram Trigger Node**: Update with your Telegram credentials
2. **Google TextSearch Node**: Verify the API endpoint and field mask
3. **Excel'e Kaydetme Node**: Update the Google Sheet document ID and sheet name

### 4. Activate the Workflow

1. Click on the "Activate" toggle to enable the workflow
2. Share your Telegram bot link with users who should be able to trigger searches

## How It Works

### Telegram Integration
Users can send a message to the Telegram bot with a search query (e.g., "restaurants in Istanbul"). The workflow will:
1. Capture the message text
2. Use it as the search query for Google Places API
3. Return the results in the Google Sheet

### Google Places API Integration
The workflow uses the Google Places Text Search API to find places matching the query. It includes:
- Proper API headers with the API key
- Field masking to retrieve only necessary data
- Pagination handling for results with multiple pages

### Data Processing
The workflow extracts the following information from each place:
- Name (displayName)
- Formatted address
- Phone number
- Website URL
- Latitude and longitude
- Rating
- Google Maps URL
- Opening hours

### Google Sheets Integration
Results are saved to a Google Sheet with the following columns:
- Name (İşletme Adı)
- Address (Adres)
- Phone (Telefon)
- Website (Web Sitesi)
- Latitude (Enlem)
- Longitude (Boylam)
- Opening Hours (acilis_saatleri)
- Rating
- Google Maps URL

## Workflow Nodes

1. **Telegram Trigger**: Listens for Telegram messages
2. **Set Region**: Sets region and alcohol filter parameters
3. **Build Query**: Constructs the search query from Telegram message
4. **Google TextSearch**: Calls Google Places API
5. **Has Next Page?**: Checks if there are more pages of results
6. **Wait for Pagination**: Waits before requesting next page
7. **Combine Results**: Merges results from multiple pages
8. **SplitInBatches**: Processes places in batches
9. **Wait for Rate Limit**: Respects API rate limits
10. **Code in JavaScript**: Processes and formats the data
11. **Excel'e Kaydetme**: Saves data to Google Sheets

## Customization

### Modify Search Parameters
To change what data is searched for, modify the "Build Query" node:
- Update the textQuery value to change how queries are constructed
- Add additional parameters for more specific searches

### Add/Remove Data Fields
To include additional place data:
1. Modify the X-Goog-FieldMask header in the Google TextSearch node
2. Update the JavaScript code in "Code in JavaScript" node to process new fields
3. Add new columns to the Google Sheets mapping

### Adjust Rate Limits
Modify the wait times in:
- "Wait for Pagination" node (default: 2 seconds)
- "Wait for Rate Limit" node (default: 0.2 seconds)

## Troubleshooting

### Common Issues

1. **API Key Errors**
   - Ensure your Google Places API key is valid
   - Check that Places API is enabled for your key
   - Verify the API key has proper restrictions

2. **Telegram Integration Issues**
   - Confirm your Telegram bot token is correct
   - Check that the webhook URL is accessible
   - Ensure the bot is added to the chat

3. **Google Sheets Errors**
   - Verify OAuth permissions are granted
   - Check that the document ID is correct
   - Ensure the sheet name exists in the document

### Logs and Debugging
- Use n8n's built-in execution logs to trace errors
- Enable "Save Manual Executions" to test changes
- Check node-specific error messages for detailed information

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a pull request

## Contact

For questions or support, please open an issue in this repository.
