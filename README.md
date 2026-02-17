# gh-action-explorer

Automated screenshot capture of a live website using GitHub Actions and shot-scraper.

## Overview

This repository automatically takes screenshots of [https://cynthiateeters.github.io/gh-action-explorer/](https://cynthiateeters.github.io/gh-action-explorer/) using GitHub Actions. Screenshots are captured on every push and stored in the repository, creating a visual history of the site over time.

## Live site

[https://cynthiateeters.github.io/gh-action-explorer/](https://cynthiateeters.github.io/gh-action-explorer/)

## How it works

The GitHub Action workflow:

1. Runs on every push or can be triggered manually
2. Uses [shot-scraper](https://github.com/simonw/shot-scraper) to capture screenshots
3. Automatically commits and pushes the updated screenshot(s)
4. Maintains a full git history of all screenshot versions

## Running the action

### Automatic

The action runs automatically on every push to the repository.

### Manual

1. Go to the **Actions** tab
2. Select **Take screenshots** workflow
3. Click **Run workflow**
4. Click the green **Run workflow** button

## Configuring screenshots

Edit `shots.yml` to customize your screenshots:

```yaml
- url: https://cynthiateeters.github.io/gh-action-explorer/
  output: shot.png
  height: 800
```

### Available options

- `output` - Filename for the screenshot
- `height` - Set a specific height (remove for full-page screenshot)
- `width` - Set a specific width
- `wait` - Wait time in milliseconds before capturing (e.g., `wait: 2000`)
- `quality` - JPEG quality 1-100 (e.g., `quality: 80`)
- `javascript` - Execute JavaScript before taking the screenshot

### Example with custom JavaScript

```yaml
- url: https://example.com/
  output: example.jpg
  width: 1600
  height: 1200
  quality: 80
  wait: 2000
  javascript: |
    document.querySelector('.modal').style.display = 'none'
```

### Multiple screenshots

Add additional entries to capture multiple pages:

```yaml
- url: https://cynthiateeters.github.io/gh-action-explorer/
  output: shot.png
  height: 800

- url: https://example.com/
  output: example.png
  height: 600
```

## Beginner's guide

### What is this project?

This project demonstrates how to use GitHub Actions to automatically take screenshots of a website. Every time you push changes to the repository, GitHub runs a script that:

1. Opens your website in a headless browser
2. Takes a screenshot
3. Saves the screenshot to your repository
4. Commits the changes automatically

This creates a visual timeline of your website's appearance over time.

### Getting started

1. **Fork or clone this repository**
2. **Update `shots.yml`** with your website URL
3. **Commit and push** - The action will run automatically
4. **Check the Actions tab** to see the workflow progress
5. **View the screenshot** - After the action completes, `shot.png` will appear in your repository

### Understanding the files

- `.github/workflows/shots.yml` - The GitHub Actions workflow configuration
- `shots.yml` - Configuration file that defines what screenshots to take
- `requirements.txt` - Python package dependencies file. This is a standard pip (Python's package installer) convention that lists all Python packages needed for the project. When the workflow runs `pip install -r requirements.txt`, it automatically installs all listed packages (in this case, shot-scraper). This file is typically kept in the repository root following Python community conventions.
- `shot.png` - The captured screenshot (created by the action)

### Troubleshooting

- **Action not running?** Check the Actions tab for error messages
- **Screenshot looks wrong?** Try adding a `wait` parameter to give the page more time to load
- **Need fonts for other languages?** See the [shot-scraper-template documentation](https://github.com/simonw/shot-scraper-template#installing-fonts-for-more-languages)

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a new branch for your feature (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Commit your changes (`git commit -m 'Add some amazing feature'`)
5. Push to the branch (`git push origin feature/amazing-feature`)
6. Open a Pull Request

Please ensure your code follows the existing style and includes appropriate documentation.

## Resources

- [shot-scraper documentation](https://github.com/simonw/shot-scraper)
- [shot-scraper-template](https://github.com/simonw/shot-scraper-template)
- [GitHub Actions documentation](https://docs.github.com/en/actions)

## License

MIT License

Copyright (c) 2026 Cynthia Teeters

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
