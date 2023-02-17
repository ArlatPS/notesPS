# Styled Components

```
npm install --save styled- components @types/styled-components

import styled from 'styled-components"
```

- creating elements

  ```
  const StyledDiv = styled.div`
      width: 1000px;
  `

  <StyledDiv>
      ...
  </StyledDiv>
  ```

- can nest 🎉
  ```
  const StyledDiv = styled.div`
      width: 1000px;
      h1 {
          color: purple;
      }
      &:hover {
          width: 2000px;
      }
  `
  ```
- passing props
  ```
  <StyledDiv bg="red">
  const StyledDiv = styled.div`
      background: ${(props) => props.bg}
  `
  ```
- themes

  ```
  import { ThemeProvider } from "styled-components"

  const theme = {
      colors: {
          primary: blue;
      }
  }

  <ThemeProvider theme={theme}>
      everything that wants access
  </Theme Provider>
  const StyledDiv = styled.div`
      background: ${({theme}) => theme.colors.primary}
  `

  ```

- global styles

  ```
  //Global.js
  import { createGlobalStyle } from "styled-components"

  const GlobalStyles = createGlobalStyle`
      html {
          box-sizing: border-box;
      }
      *,*::before, *::after {
          box-sizing: inherit;
      }
  `
  export default GlobalStyles

  //in main js at the top
  <GlobalStyles />
  ```
