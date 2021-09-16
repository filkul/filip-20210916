# My Trading Interface

This repository holds the code for a modern trading interface. It uses [yarn workspaces](https://classic.yarnpkg.com/en/docs/workspaces/) to handle multiple packages. Packages-specific documentation can be found within the particular folders of the packages.

## Getting started

```sh
# First, install all dependencies:
yarn install

# Then build the required packages:
yarn build

# Finally run the development server:
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

## Structure

- `my-trading-interface` - main code of the trading interface
- `packages` - packages used by the trading interface

## Choices

### CSS

My first choice for handling CSS is normally `styled-components`, but since performance was key for this project I've decided to go for a build-time CSS in JS solution (recommended article: [Real-world CSS vs. CSS-in-JS performance comparison](https://pustelto.com/blog/css-vs-css-in-js-perf/)). I've tested two most popular libraries out there: [linaria](https://github.com/callstack/linaria) and [compiled](https://github.com/atlassian-labs/compiled) by Atlassian Labs and even though the latter felt more production-ready, it's still relatively young and has some [major features missing](https://compiledcssinjs.com/docs/limitations#missing-behavior). This is why I stuck to linaria.

### Detecting page visibility and media queries

I've decided to move the logic for detecting the page visibility and screen width into a higher-order component. This is because I can easily imagine other elements of the trading interface (`widgets`) making use of those features. 

I'm using mature libraries: [react-page-visibility](https://github.com/pgilad/react-page-visibility) for page visibility detection and [react-responsive](https://github.com/contra/react-responsive) for media queries. Having information about the screen width in JS instead of only in CSS allows to additionally trim the Orderbook entries for better performance.

### Moving logic into packages

Moving parts of the code into packages (@fkul/avg and @fkul/react-ws-api) helps to better separate presentation from logic. It also allows to reuse the packages in other projects.

### Throttling

I'm using [lodash/throttle](https://lodash.com/docs/4.17.15#throttle) to perform the throttling itself, but the logic to calculate the wait time is based on the average time that the Orderbook's content is generated by the browser. There's a known caveat about that approach described below.

## Caveats

- linaria is not able to use variables in globals, making theming harder
- RWD breakpoints are hardcoded, which is also result of linaria's limitations
- If the Orderbook's generation time hover around the threshold for switching throttle wait time, the switching can occur more frequent than it should

## Testing

The command below will run tests in every workspace of the repository:

```sh
yarn test
```
