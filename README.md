html2xlsx-hd
===========

Based on [html2xlsx](https://github.com/d-band/html2xlsx) with change [data-base64-xlsx-cell-config](https://github.com/d-band/html2xlsx/pull/64)

> Transform html to excel (just support xlsx)

[![NPM version](https://img.shields.io/npm/v/html2xlsx.svg)](https://www.npmjs.com/package/html2xlsx-hd)
[![NPM downloads](https://img.shields.io/npm/dm/html2xlsx-hd.svg)](https://www.npmjs.com/package/html2xlsx-hd)
[![Build Status](https://travis-ci.org/yuanoook/html2xlsx-hd.svg?branch=master)](https://travis-ci.org/yuanoook/html2xlsx-hd)
[![Coverage Status](https://coveralls.io/repos/github/yuanoook/html2xlsx-hd/badge.svg?branch=master)](https://coveralls.io/github/yuanoook/html2xlsx-hd?branch=master)
[![Dependency Status](https://david-dm.org/yuanoook/html2xlsx-hd.svg)](https://david-dm.org/yuanoook/html2xlsx-hd)
[![Greenkeeper badge](https://badges.greenkeeper.io/yuanoook/html2xlsx-hd.svg)](https://greenkeeper.io/)

---

## Install

```bash
$ npm install html2xlsx-hd
```

## Usage

- [More Examples](examples)

```javascript
const fs = require('fs');
const htmlTo = require('html2xlsx-hd');

htmlTo(`
  <style type="text/css">
    table td {
      color: #666;
      height: 20px;
      background-color: #f1f1f1;
      border: 1px solid #eee;
    }
  </style>
  <table>
    <tr>
      <td>foo</td>
      <td>bar</td>
    </tr>
    <tr>
      <td>hello</td>
      <td>world</td>
    </tr>
    <tr>
      <td type="number">123</td>
      <td type="number">123.456</td>
    </tr>
    <tr>
      <td data-type="bool">true</td>
      <td data-type="bool">false</td>
    </tr>
    <tr>
      <td data-type="bool">1</td>
      <td data-type="bool">0</td>
    </tr>
    <tr>
      <td type="formula">SUM(A1:B1)</td>
      <td type="formula">A1-B1</td>
    </tr>
    <tr>
      <td type="date">2013-01-12T12:34:56+08:00</td>
      <td type="datetime">2013-01-12T12:34:56+08:00</td>
    </tr>
  </table>
`, (err, file) => {
  if (err) return console.error(err);
  
  file.saveAs()
    .pipe(fs.createWriteStream('test.xlsx'))
    .on('finish', () => console.log('Done.'));
});
```

## Customize cell

Config a cell with all possible _value, formula, numFmt, cellType...

To use `data-base64-xlsx-cell-config`

```javascript
const htmlTo = require('html2xlsx-hd')
const btoa = (window && window.btoa) || (str => Buffer.from(str, 'utf-8').toString('base64'))

htmlTo(`
  <table>
    <tr>
      <td data-base64-xlsx-cell-config="${btoa(JSON.stringify({
          numFmt: '"$"#,##0.00;[Red]\-"$"#,##0.00',
          cellType: 'TypeNumeric',
          _value: 10000
        }))}"
      >
        10000
      </td>
    </tr>
  </table>
`, (err, file) => {
  if (err) return console.error(err);

  file.saveAs()
    .pipe(fs.createWriteStream('test.xlsx'))
    .on('finish', () => console.log('Done.'));
});
```

## Report a issue

* [All issues](https://github.com/yuanoook/html2xlsx-hd/issues)
* [New issue](https://github.com/yuanoook/html2xlsx-hd/issues/new)

## Reference

- https://github.com/d-band/html2xlsx
- https://github.com/d-band/better-xlsx

## License

html2xlsx-hd is available under the terms of the MIT License.
