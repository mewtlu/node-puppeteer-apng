# Node Puppeteer APNG

A pure JavaScript Node library for recording a screencast of a webpage and
returning an animated PNG (APNG) file of the screencast. APNG is supported
in Firefox, Chrome, Safari and in the Chrome-based version of Edge. Also,
GitHub will play animated PNGs in readmes and other MarkDown documents, no
problem.

## Usage and API

`npm install https://github.com/TomasHubelbauer/node-puppeteer-apng`

### Capture a 5 second clip of a webpage by URL

```js
const buffer = await nodePuppeteerApng('https://google.com/ncr');
await fs.writeFile(`screencast.png`, buffer);
```

### Set up a Puppeteer scene and start and stop the recording programatically

```js
const buffer = await nodePuppeteerApng(async (start, stop) => {
  const browser = await puppeteer.launch();
  const [page] = await browser.pages();
  await page.goto('https://www.youtube.com/watch?v=jNQXAC9IVRw');
  const playButton = await page.waitForSelector('.ytp-large-play-button');
  await playButton.click();
  await start(page);
  await page.waitFor(5000);
  await stop();
  await browser.close();
});

await fs.writeFile(`screencast.png`, buffer);
```

## To-Do

### Set up GitHub Actions for the test

### Rename cuts to timestampts everywhere

### Fix the timestamps in the trace method

### Fix the flaky custom setup test failing on the play button

### Figure out why the `startScreencast` method has layers shifted

And if I can do something about that or if it is a Puppeteer thing.

### Resize the window to match the viewport for the screencast one

### Deploy to NPM

### Consider if the image could be quantized and paletted programatically

Without shelling out to native binaries.
Probably the best way to do this would be to study how the quantization and
palette generation is done in PNG, then scan all the PNGs and collect all the
colors, quantize them and replace them in the true color PNG, then rewrite all
the individual PNGs to refer to the same palette, then keep the palette from
the first PNG knowing the subsequent ones will have correct indices into it as
well.
