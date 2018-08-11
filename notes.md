# Web Scraping with Cheerio

Install cheerio and request

```shell
npm i cheerio request
```

### scrape.js
---
```js
const request = require('request');
const cheerio = require('cheerio');

request('http://codedemos.com/sampleblog/', (error, response, html) => {
  if (!error && response.statusCode === 200) {
    const $ = cheerio.load(html);

    const siteHeading = $('.site-heading');

    // console.log(siteHeading.html());
    
    // console.log(siteHeading.text());
    
    // const output = siteHeading.find('h1').text();
    
    // const output = siteHeading.children('h1').text();

    // const output = siteHeading
    //   .children('h1')
    //   .next()
    //   .text();
    
    // const output = siteHeading
    //   .children('h1')
    //   .parent()
    //   .text();

    $('.nav-item a').each((i, el) => {
      const item = $(el).text();
      const link = $(el).attr('href');

      console.log(link);
    });
  }
});
```

### scrape2.js
---
```js
const request = require('request');
const cheerio = require('cheerio');
const fs = require('fs');
const writeStream = fs.createWriteStream('post.csv');

// Write headers
writeStream.write('Title, Link, Date \n');

request('http://codedemos.com/sampleblog/', (error, response, html) => {
  if (!error && response.statusCode === 200) {
    const $ = cheerio.load(html);

    $('.post-preview').each((i, el) => {
      const title = $(el)
        .find('.post-title')
        .text()
        .replace(/\s\s+/g, ''); // get rid of whitespace that's not between words

      const link = $(el)
        .find('a')
        .attr('href');

      const date = $(el)
        .find('.post-date')
        .text()
        .replace(/,/, ''); // replace comma with a space

        // Write row to CSV
        writeStream.write(`${title}, ${link}, ${date}`);
    });

    console.log('Scraping done...');
  }
});
```