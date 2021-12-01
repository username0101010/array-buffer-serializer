# array-buffer-serializer

![CI](https://github.com/username0101010/array-buffer-serializer/actions/workflows/test.yml/badge.svg)
[![codecov](https://codecov.io/gh/username0101010/array-buffer-serializer/branch/main/graph/badge.svg?token=IZFQQP34H7)](https://codecov.io/gh/username0101010/array-buffer-serializer)

Allows to encode an object into bytes before transmission via WebRTC or WebSocket and decode it back when received. 
This is very useful for network bandwidth when you need to send data to one or more recipients frequently.

Some characters of the [Latin-1 Supplement](https://en.wikipedia.org/wiki/Latin-1_Supplement_(Unicode_block))
are reserved for encoding values: C0 (U+00C0) - E3 (U+00E7).

## Installation

Using npm:

```bash
npm install array-buffer-serializer
```

With yarn:

```bash
yarn add array-buffer-serializer
```

## Usage

1. **Import Serializer object**
    
    ```javascript
    // as CommonJS
    const Serializer = require("array-buffer-serializer");
    // or ES6
    import Serializer from "array-buffer-serializer";
    ```
    
2. **Encode some data (object or array) using "toBuffer" method**

    ```javascript
    // as object
    const data = { this: "way" };
    // as array
    const data = ["or", "that", "way"];
    
    const buffer = Serializer.toBuffer(data);
    ```

3. **Send the buffer (for example, over WebScoket)**     
    ```javascript
    ws.send(buffer);
    ```
    
4. **Receive the buffer and decode it using "fromBuffer" method**

    ```javascript
    ws.addEventListener("message", buffer => {
        const data = Serializer.fromBuffer(buffer);
    });
    ```

## Table

<table>
    <tr>
        <th rowspan="2">Type</th>
        <th colspan="2">Data</th>
        <th colspan="2">Bytes</th>
    </tr>
    <tr>
	<th>Object</th>
	<th>Raw</th>
        <th>Object</th>
        <th>Raw</th>
    </tr>
    <tr>
        <td>undefined</td>
		<td>[undefined]</td>
		<td><00 d4></td>
        <td>11</td>
		<td>2</td>
    </tr>
    <tr>
        <td>true</td>
		<td>[true]</td>
		<td><00 d2></td>
        <td>6</td>
		<td>2</td>
    </tr>
    <tr>
        <td>false</td>
		<td>[false]</td>
		<td><00 d0></td>
        <td>7</td>
		<td>2</td>
    </tr>
    <tr>
        <td>null</td>
		<td>[null]</td>
		<td><00 d6></td>
        <td>6</td>
		<td>2</td>
    </tr>
    <tr>
        <td>string</td>
		<td>["text"]</td>
		<td><00 dc 74 65 78 74 dc></td>
        <td>8</td>
		<td>7</td>
    </tr>
    <tr>
        <td>int8_t</td>
		<td>[255]</td>
		<td><00 c0 ff></td>
        <td>5</td>
		<td>3</td>
    </tr>
    <tr>
        <td>int16_t</td>
		<td>[-65535]</td>
		<td><00 c6 ff ff></td>
        <td>8</td>
		<td>4</td>
    </tr>
    <tr>
        <td>int32_t</td>
		<td>[4294967295]</td>
		<td><00 c8 ff ff ff ff></td>
        <td>12</td>
		<td>6</td>
    </tr>
    <tr>
        <td>bigint</td>
		<td>[-18446744073709551615n]</td>
		<td><00 e2 ff ff ff ff ff ff ff ff></td>
        <td>24</td>
		<td>10</td>
    </tr>
    <tr>
        <td>float</td>
		<td>[1.234567]</td>
		<td><00 d8 3f 9e 06 4b></td>
        <td>10</td>
		<td>6</td>
    </tr>
    <tr>
        <td>double</td>
		<td>[1.23456789012]</td>
		<td><00 da 3f f3 c0 ca 42 8c 1d 2b></td>
        <td>15</td>
		<td>10</td>
    </tr>
</table>

## Features

* No model is needed to describe the data structure
* Uses type definition instead of object's key-value separator ":" and array items delimiter ","
* Uses unsigned data representation by default (uint8_t, uint16_t...)
* Different marks for positive and negative numbers, so negative sign is for free
* Marks are divided into even and odd, each odd mark indicates at the first array value

## License

[MIT](./LICENSE)
