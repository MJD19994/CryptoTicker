# Crypto Tracker for Adafruit Matrix Portal S3

A CircuitPython application that displays cryptocurrency prices and 30-day price history graphs on a 64x64 RGB matrix panel, with a responsive web dashboard for managing tracked coins.

## Features

- **Matrix Display**: Real-time cryptocurrency price display with beautiful line graphs
- **Automatic Cycling**: Cycles through tracked coins every 30 seconds
- **Web Dashboard**: Responsive web GUI for adding/removing coins and viewing current prices
- **30-Day History**: Fetches and displays historical price data from CoinGecko API
- **Price Alerts**: 24-hour percentage change indicators (green for gains, red for losses)
- **Persistent Configuration**: Coin selections saved to local config file
- **Auto-Reconnect**: Automatic WiFi reconnection on connection loss
- **Responsive Design**: Web dashboard works on desktop, tablet, and mobile devices

## Hardware Requirements

- Adafruit Matrix Portal S3
- 64x64 RGB LED Matrix Panel
- USB-C power adapter
- WiFi network connectivity

## Installation

1. Install CircuitPython 9.2.7+ on your Matrix Portal S3
2. Download the Adafruit CircuitPython library bundle
3. Copy required libraries to `/lib`:
   - `adafruit_httpserver/` (all modules)
   - `adafruit_requests.mpy`
   - `adafruit_ntp.mpy`
   - `adafruit_display_text/` (label module)
4. Place these files on the root of your device:
   - `code.py`
   - `dashboard.html`
   - `settings.toml` (update with your WiFi credentials)
   - `coin_config.txt`

## Configuration

### WiFi Setup (`settings.toml`)

```toml
CIRCUITPY_WIFI_SSID = "Your WiFi SSID"
CIRCUITPY_WIFI_PASSWORD = "Your WiFi Password"

# Optional: CircuitPython Web API (on port 8080)
CIRCUITPY_WEB_API_PASSWORD = "passw0rd"
CIRCUITPY_WEB_API_PORT = 8080
```

### Tracked Coins (`coin_config.txt`)

Format: `SYMBOL, coingecko_id`

Example:
```
BTC, bitcoin
ETH, ethereum
DOGE, dogecoin
XRP, ripple
```

Find CoinGecko IDs at: https://api.coingecko.com/api/v3/coins/list

## Usage

1. **Power on the device** - It will connect to WiFi automatically
2. **View on Matrix** - Prices cycle through automatically every 30 seconds
3. **Access Dashboard** - Open your browser and go to `http://<device_ip>`
   - Find your device IP in the serial console output
   - Default: `http://192.168.1.218` (may vary on your network)

### Dashboard Features

- **Add New Coin**: Enter symbol (e.g., BTC) and CoinGecko ID (e.g., bitcoin)
- **Remove Coin**: Click "Remove" button next to any tracked coin
- **Refresh Prices**: Manually fetch latest price data
- **Current Prices**: View all tracked coins with 24-hour percentage changes

## API Endpoints

The web server provides JSON API endpoints:

- `GET /api/coins` - Get list of tracked coins
- `POST /api/coins` - Add a new coin
  - Body: `{"symbol": "BTC", "id": "bitcoin"}`
- `DELETE /api/coins/<symbol>` - Remove a coin
- `GET /api/prices` - Get current prices and 24h changes

## Data Source

Price data is fetched from the **CoinGecko API** (free tier, no API key required).

- Current prices updated every 6 minutes
- 30-day historical data fetched with each update
- Rate limiting: ~5 requests per minute recommended

## Performance Notes

- Memory usage: ~2MB available for heap
- Display refresh: 30 second intervals per coin
- Web server polling: 0.1 second intervals during display
- Matrix update rate: 125 Hz (built-in)

## Troubleshooting

**"Site unreachable"**
- Check IP address in serial console
- Ensure device is connected to WiFi
- Try `http://192.168.1.218` (adjust IP for your network)

**"Read-only filesystem" errors**
- Disconnect USB cable from computer
- The filesystem becomes read-only when connected to CircuitPython editor

**Missing prices**
- Check internet connectivity
- May be rate-limited by CoinGecko API
- Wait a few minutes before retrying

**Matrix not displaying**
- Verify GPIO pin configuration matches your setup
- Check RGB matrix panel connections
- See pin assignments in `code.py` lines 71-84

## Project Structure

```
├── code.py                 # Main application
├── dashboard.html          # Web GUI (separate from code)
├── coin_config.txt         # Tracked coins configuration
├── settings.toml           # WiFi and system settings
├── lib/                    # Adafruit libraries
│   ├── adafruit_httpserver/
│   ├── adafruit_requests.mpy
│   ├── adafruit_ntp.mpy
│   └── adafruit_display_text/
└── README.md              # This file
```

## Development

### Key Functions

- `fetch_crypto_prices()` - Get current prices from CoinGecko
- `fetch_price_history()` - Get 30-day historical data
- `draw_price_history_graph()` - Render graph on matrix
- `update_display()` - Update matrix with coin data
- `setup_web_server()` - Initialize HTTP server with routes
- `get_html_dashboard()` - Load web GUI from dashboard.html

### Customization Ideas

- Change coins displayed with coin_config.txt
- Modify display update interval (30 seconds in main loop)
- Adjust graph height (GRAPH_HEIGHT variable)
- Customize web dashboard colors in dashboard.html
- Add price alerts or notifications

## License

MIT License - Feel free to modify and distribute

## Credits

Built with:
- [CircuitPython](https://circuitpython.org/)
- [Adafruit Libraries](https://github.com/adafruit)
- [CoinGecko API](https://www.coingecko.com/api)

## Support

For issues, questions, or contributions, please open an issue on GitHub.

---

**Happy tracking! 📊💰**
