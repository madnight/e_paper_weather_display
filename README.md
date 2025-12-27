

# E-paper Weather Display

This project uses a Raspberry Pi 2 Zero WH (no soldering required) to show weather updates and trash reminders on a Waveshare 7.5-inch e-paper display. It fetches weather data from OpenWeatherMap and refreshes the display at set intervals. Minimal energy consumption makes this setup ideal for continuous display without frequent updates.

![Display Photo 1](https://github.com/AbnormalDistributions/e_paper_weather_display/blob/master/photos/photo1.jpg?raw=true)
![Display Photo 2](https://raw.githubusercontent.com/AbnormalDistributions/e_paper_weather_display/refs/heads/master/photos/photo2.jpg)

---

## Parts List
- **Waveshare 7.5-inch e-Paper HAT**: [Available on Amazon](https://amzn.to/3UBxuah) (affiliate link).  
- **Raspberry Pi 2 Zero WH ** (any model should work except the Pi Zero without soldered headers).
- **SD Card**
- **Power supply** for the Raspberry Pi.

## Setup Instructions

### Installation
1. **Clone the Project**:
   Open a terminal and run:
   ```bash
   sudo apt-get install git
   git clone https://github.com/AbnormalDistributions/e_paper_weather_display.git
   git clone https://github.com/waveshare/e-Paper.git
   cd e_paper_weather_display
   ```

2. **Install Python Libraries**:
   ```bash
   sudo apt install python3-pillow python3-requests python3-pil
   ```

### Configuration
1. **Add Your OpenWeatherMap API Key**:
   Sign up on [OpenWeatherMap](https://home.openweathermap.org/users/sign_up) for an API key, then open `weather.py` and add your API key where it says `API_KEY = 'XXXXXXXX'`.

2. **Settings**:
   Edit the following user-defined settings at the top of `weather.py`:

   Replace this line in weather.py
   ```py
   lib_path = os.path.join(script_dir, 'lib')
   ```
   with
   ```py
   lib_path = "/home/pi/e-Paper/RaspberryPi_JetsonNano/python/lib" # Set according to your git download
   ```

   - `API_KEY`: Your OpenWeatherMap 2.5 API key.
   - `LOCATION`: Name of the location to display (e.g., `New Orleans`).
   - `LATITUDE` and `LONGITUDE`: Coordinates for weather updates (use [Google Maps](https://maps.google.com) to find these).
   - `UNITS`: Choose `'imperial'` (Fahrenheit) or `'metric'` (Celsius).
   - `CSV_OPTION`: Set this to `True` if you’d like to save a daily log of weather data in `records.csv`.
   - `TRASH_DAYS`: Add the days for trash reminders as numbers (0=Monday, 6=Sunday).

> **Note**: If you are not using a 7.5 inch Version 2 display, you will want to replace 'epd7in5_V2.py' in the 'lib' folder with the appropriate version from [Waveshare's e-Paper library](https://github.com/waveshare/e-Paper/tree/master/RaspberryPi_JetsonNano/python/lib/waveshare_epd). Adjustments will be required for other screen sizes.

## Running the Script
1. **To Run Manually**:
   From the `e_paper_weather_display` directory, run:
   ```bash
   python weather.py
   ```
   This will fetch the weather data and update the display immediately.

## Setting up Automatic Updates
Set up a scheduled update every 15 minutes using `crontab`. This will make sure your display updates automatically.

In the terminal, type:
```bash
crontab -e
```
Then, add the following line at the end of the file:
```bash
*/15 * * * * /usr/bin/python /home/pi/e_paper_weather_display/weather.py >> /home/pi/e_paper_weather_display/weather_display.log 2>&1
```
- This command updates the display every 15 minutes.
- Be sure to replace `/home/pi/e_paper_weather_display/` with the path where the project is stored, if different.

## Credit and License
- Icon designs by [Erik Flowers](https://erikflowers.github.io/weather-icons/), with some modifications.
- **Weather Icons**: Licensed under [SIL OFL 1.1](http://scripts.sil.org/OFL).
- **Code**: Licensed under [MIT License](http://opensource.org/licenses/mit-license.html).
- **Documentation**: Licensed under [CC BY 3.0](http://creativecommons.org/licenses/by/3.0).
