# Vjetar: which wind is it?

A single-page website that tells you the traditional Croatian name of the wind blowing in a coastal town right now: bura, jugo, maestral, and so on.

Type a coastal Croatian town or city and the page shows the wind's name, a short description, speed, gusts, direction and Beaufort number, plus a wind rose with the current direction highlighted. It's one static HTML file with no build step and no API key.

## How it works

1. You enter a town. The page looks it up with the [Open-Meteo geocoding API](https://open-meteo.com/en/docs/geocoding-api), limited to Croatia. If several places share the name, you pick one from a short list.
2. It fetches the current wind speed, gusts and direction (at 10 m) from the [Open-Meteo forecast API](https://open-meteo.com/en/docs).
3. It converts the direction to a name, using the season and time of day where the wind depends on them.
4. It refreshes every 10 minutes and whenever you return to the tab. Your last town, speed unit and language are remembered in your browser.

### How the wind is named

The name follows the direction the wind blows **from**:

| From | Name |
|---|---|
| N | Tramontana |
| NE | Bura |
| E | Levant |
| SE | Jugo |
| S | Oštro |
| SW | Lebić |
| W | Pulenat |
| NW | Maestral |

A few refinements:

- **Bonaca**: under 3 knots, shown as calm.
- **Maestral** is a daytime sea breeze of the warm season. It is shown only for a northwest wind between April and October and between 10:00 and 20:00 local time. Any other northwest wind is shown as *Sjeverozapadni vjetar* (northwest wind).
- **Burin**: a light northeast wind (under about 13 knots) at night in the warm season. By day, or in winter, a light northeast wind is shown as Bura.

These rules are a simplification. Sources on Adriatic winds differ on exact seasons and hours, and local geography matters. The thresholds are in the `classify` function in the script, so they are easy to change.

## Run it locally

Open `index.html` in a browser. It needs an internet connection for the two Open-Meteo requests. The Google Font is optional, and the page falls back to a system font without it.

## Publish on GitHub Pages

1. Put `index.html` in the root of a public repository.
2. In the repository, go to **Settings → Pages**, choose **Deploy from a branch**, select `main` and `/ (root)`, and save.
3. After a minute or two the site is live at `https://<username>.github.io/<repository>/`.

## Customising

- **Starting town**: the page asks for a town on the first visit. To preset one, set a default place object in the script and pass it to `setPlace` in `init`.
- **Naming rules and descriptions**: edit the `WINDS` table and the `classify` function.
- **Language**: an EN | HR switch in the header changes the interface, descriptions and compass letters. English is the default, and the choice is remembered. The wind names themselves (bura, jugo, maestral and so on) stay Croatian in both languages. All text lives in the `I18N` object and the `WINDS` table in the script.

## Limitations

- The numbers come from weather-model data for the grid cell around the town, not from a measuring station there. Local effects, such as gusts funnelling through a channel in a bura, can differ from what the page shows.
- The naming rules are tuned for the coast and islands. For inland towns, a coastal wind name may not be meaningful.
- Open-Meteo's free tier is for non-commercial use. A commercial deployment needs their paid plan.

## Credits

Weather and location data: [Open-Meteo](https://open-meteo.com).
