# Carnivorous Plant Show Guide - Project Memory

## Project Overview
A comprehensive guide website for the SCCPE Carnivorous Plant Show held at Norma Hertzog Community Center in Costa Mesa, CA. Features detailed parking information with interactive Google Maps integration.

## Critical Technical Details

### Venue Location
- **Address**: 1845 Park Avenue, Costa Mesa, CA 92627
- **Coordinates**: 33.641959, -117.922051 (EXACT - DO NOT CHANGE)
- **Name**: Norma Hertzog Community Center

### Google Maps Integration

#### API Keys
- Use `get-secret google-api-echo2` to retrieve API key
- APIs enabled: Maps JavaScript, Places, Geocoding, Streets View, Roads, Directions, Distance Matrix

#### CRITICAL: Map Coordinate Precision
**ALWAYS USE 7 DECIMAL PLACES FOR LAT/LNG COORDINATES**
- NEVER use trailing zeros (e.g., 33.6415000 is WRONG)
- ALWAYS use meaningful precision (e.g., 33.6415234 is CORRECT)
- At zoom 18, each 0.0001 degree ≈ 11 meters

#### How to Get Accurate Street Coordinates
**NEVER GUESS COORDINATES! Always use the Google Maps API:**

```bash
# Get intersection coordinates
curl -s "https://maps.googleapis.com/maps/api/geocode/json?address=W+18th+Street+and+Park+Avenue+Costa+Mesa+CA&key=$(get-secret google-api-echo2)" | jq '.results[0].geometry.location'

# Get street coordinates
curl -s "https://maps.googleapis.com/maps/api/geocode/json?address=Park+Avenue+Costa+Mesa+CA+92627&key=$(get-secret google-api-echo2)" | jq '.results[0].geometry.location'
```

### Actual Street Layout (FROM API)
- **W 18th St & Park Ave**: 33.6398703, -117.9211038
- **W 19th St & Park Ave**: 33.6435017, -117.9211378
- **Park Avenue**: Runs north-south at longitude -117.9211346
- **Venue Parking Lot**: East side of building (not a street)

### Parking Zones Structure
1. **Zone 1 - Park Ave**: Street parking (2-point polyline)
2. **Zone 2 - W 18th St**: South of venue (2-point polyline)
3. **Zone 3 - W 19th St**: North of venue (2-point polyline)
4. **Zone 4 - Venue Lot**: East of building (5-point polygon)

### Common Mistakes to Avoid
1. **DON'T** use coordinates with trailing zeros (kills precision)
2. **DON'T** guess street locations - always use API
3. **DON'T** assume Monrovia Ave exists (it doesn't - just parking lots)
4. **DON'T** place zones at arbitrary offsets - use actual geocoded positions
5. **DON'T** change venue location from 33.641959, -117.922051

### Map Display Settings
- Default view: Satellite
- Zoom level: 18
- Center: Venue location
- Show: Venue marker, building rectangle, parking zones, legend

### Street View Images
- Located in `images/parking/`
- park-18th.jpg: View looking north on Park Ave
- park-19th.jpg: View from Park & 19th (Bank of America visible)

## Development Notes
- Use `python3 -m http.server` for local testing
- GitHub Pages deployment: https://eugenesandugey.github.io/carnivorous-plant-show-guide/
- Always commit with descriptive messages
- Push changes immediately for live updates

## Testing Coordinates
When updating parking zones, always:
1. Use Google Maps API to get exact coordinates
2. Verify precision (7 meaningful decimal places)
3. Test locally before pushing
4. Check satellite view to confirm placement

## Remember
The user spent HOURS frustrated by wrong coordinates. ALWAYS:
- Use the Maps API for real coordinates
- Maintain proper decimal precision
- Test thoroughly before claiming it's fixed