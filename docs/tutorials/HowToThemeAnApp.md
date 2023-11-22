# How to theme an app

> Note: The theming API is still _experimental_. Names of color schemes, variants and semantic tokens are not defined yet.

This "how to" shows how to create a custom theme for an app. To learn more about the theming
mechanism in Open Pioneer Trails apps, refer to [Theming](../reference/Theming.md).

To show how a custom theme for an app can be created, we will extend the empty app
(at `src/apps/empty`).

By default, the app is shown in a theme specified in the Trails Core packages.
To overwrite some or all styles, a custom app-theme can be created.

First, in your apps "empty" folder, create a new folder called "theme" and add a file called "theme.ts".
This file will contain our theme which is a Chakra UI theming object.
The structure of this object is described in the [Chakra documentation](https://chakra-ui.com/docs/styled-system/theme).
The custom theme does not need to be a complete theme: we can just override specific theming properties.

Add the following content to your file:

```ts
// src/apps/empty/theme/theme.ts
// (1)
import { extendTheme } from "@open-pioneer/chakra-integration";
// (2)
import { theme as baseTheme } from "@open-pioneer/base-theme";

// (3)
export const theme = extendTheme(
    // (3.1)
    {
        colors: {
            primary: {
                50: "#defffd",
                100: "#b3fffa",
                200: "#86feee",
                300: "#5bfedd",
                400: "#3efec9",
                500: "#32e5a6",
                600: "#23b277",
                700: "#147f4c",
                800: "#004d23",
                900: "#001b0a"
            }
        },
        fonts: {
            heading: "Helvetica"
        },
        components: {
            Button: {
                defaultProps: {
                    colorScheme: "primary"
                },
                variants: {
                    cancel: {
                        color: "font_inverse",
                        bg: "error",
                        _hover: { backgroundColor: "error_hover" }
                    }
                }
            },
            Link: {
                baseStyle: {
                    color: "font_link"
                }
            },
            Divider: {
                baseStyle: {
                    borderColor: "border"
                }
            }
        },
        semanticTokens: {
            colors: {
                "background_primary": "primary.300",
                "background_secondary": "primary.500",
                "placeholder": "primary.100",
                "font_primary": "black",
                "font_secondary": "grey.500",
                "font_inverse": "white",
                "font_link": "yellow.300",
                "border": "black",
                "error": "red.500",
                "error_hover": "red.600",
                "success": "green.500",
                "highlight": "yellow.300",
                //override internal chakra theming variables
                "chakra-body-bg": "background_primary",
                "chakra-subtle-bg": "background_secondary",
                "chakra-body-text": "font_primary",
                "chakra-subtle-text": "font_secondary",
                "chakra-inverse-text": "font_inverse",
                "chakra-border-color": "border",
                "chakra-placeholder-color": "placeholder"
            }
        }
    },
    // (3.2)
    baseTheme
);
```

-   **(1)** Import the `extendTheme` method.
-   **(2)** Import the trails base theme from the "@open-pioneer/base-theme" package.
-   **(3)** Use `extendTheme` to create an own theme based on the trails base theme.
-   **(3.1)** Chakra UI theming object that defines the custom theme.
-   **(3.2)** Specify the theme to extend (here: the trails base theme).

To make use of the custom theme in the app, we need to modify the `app.ts`:

```ts
// src/apps/empty/app.ts
import { createCustomElement } from "@open-pioneer/runtime";
import * as appMetadata from "open-pioneer:app";
import { AppUI } from "./AppUI";
// (1)
import { theme } from "./theme/theme";

const Element = createCustomElement({
    component: AppUI,
    // (2)
    theme: theme,
    appMetadata
});

customElements.define("empty-app", Element);
```

-   **(1)** Imports the theme.
-   **(2)** Specifies `theme` as the application's theme.