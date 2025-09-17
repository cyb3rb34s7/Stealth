I'll help you research the challenge of sharing global styles between microfrontend (MFE) applications and the host/shell app when using @angular-architects/native-federation in Angular 19. Let me investigate this thoroughly.

Searched for “angular native federation share global styles microfrontend”

Searched for “native federation share global css styles between mfe angular”

Searched for “@angular-architects/native-federation share scss styles global theme”

Based on my research into sharing global styles between MFE and host applications using @angular-architects/native-federation in Angular 19, I've found that this is indeed a challenging issue with several approaches available, each with trade-offs.

## The Challenge

The core issue is that Native Federation and Module Federation are designed to keep micro frontends isolated and independent [ANGULARarchitects](https://www.angulararchitects.io/en/blog/multi-framework-and-version-micro-frontends-with-module-federation-your-4-steps-guide/) . When an MFE is loaded, global styles defined in the MFE's styles.scss don't get loaded automatically because you're typically requesting a sub-module rather than the main application [Stack Overflow](https://stackoverflow.com/questions/67633345/serving-styles-and-assets-with-webpack-5-module-federation) .

## Possible Solutions

### 1. **ViewEncapsulation.None Approach (Most Elegant)**

Import your global styles.scss of the micro frontend into its entry module component.scss and set encapsulation: ViewEncapsulation.None in the component to break style encapsulation, which will lead to those styles being applied globally [Stack Overflow](https://stackoverflow.com/questions/67633345/serving-styles-and-assets-with-webpack-5-module-federation) .

**Implementation approach:**
- Import global styles into your MFE's root component (the one you expose)
- Set ViewEncapsulation.None on that component
- The styles will then be injected when the component loads

**Pros:**
- Clean and simple
- Works with different workspaces
- No additional configuration needed

**Cons:**
- Styles affect the entire application globally
- Potential style conflicts between MFEs

### 2. **Separate CSS Bundle Approach**

Create a dedicated CSS bundle by configuring angular.json to export styles separately, then load this CSS file dynamically when the MFE is loaded [Stack Overflow](https://stackoverflow.com/questions/67633345/serving-styles-and-assets-with-webpack-5-module-federation) .

**Implementation approach:**
- Configure angular.json to create a separate CSS bundle
- Load the CSS file programmatically when loading the MFE
- Use a manifest file to track CSS URLs

**Pros:**
- Complete control over when styles are loaded
- Can unload styles when MFE is removed

**Cons:**
- More complex implementation
- Requires additional loading logic

### 3. **Shadow DOM with Style Injection**

Using Shadow DOM emulation provided by Angular allows preventing styles from bleeding while still allowing global styles to be shared [ANGULARarchitects](https://www.angulararchitects.io/en/blog/multi-framework-and-version-micro-frontends-with-module-federation-your-4-steps-guide/) . When using ViewEncapsulation.ShadowDom, you need to inject required CSS directly into the component's .scss file, including all third-party styles [Medium](https://medium.com/@epavliy/css-isolation-with-angular-micro-frontends-eb3bd8261bb9) .

**Implementation approach:**
- Use ViewEncapsulation.ShadowDom for isolation
- Inject all needed styles into the component
- Handle overlay containers for Material components

**Pros:**
- Complete style isolation
- No global pollution

**Cons:**
- Increases bundle size due to CSS duplication for each web component instance [Medium](https://medium.com/@epavliy/css-isolation-with-angular-micro-frontends-eb3bd8261bb9)
- More complex for third-party libraries

### 4. **Shared Styles in Shell Application**

Place common styles in a shared location accessible to both host and MFEs, referenced through angular.json [Itsnotbugitsfeature](https://itsnotbugitsfeature.com/2022/03/13/sharing-common-assets-in-angulars-module-federation/) .

**Implementation approach:**
- Create a shared styles folder
- Configure angular.json in both host and MFEs to reference these styles
- Use consistent paths across projects

**Pros:**
- Centralized style management
- No duplication in runtime

**Cons:**
- Requires monorepo or shared package approach
- Less flexibility for MFE-specific styles

## Recommendations

For your Angular 19 + Native Federation setup, I recommend:

1. **For truly global styles (design system, themes):** Place them in the shell/host application and ensure MFEs are designed to inherit these styles.

2. **For MFE-specific styles that need to be global:** Use the ViewEncapsulation.None approach on your exposed component, but be careful about naming conflicts.

3. **For complete isolation:** Consider Shadow DOM if you can accept the bundle size trade-off.

4. **For production robustness:** Consider implementing a hybrid approach where critical shared styles are in the host, while MFE-specific styles use ViewEncapsulation.None with careful namespacing.

## Important Considerations

- Teams often trade some independence for global CSS rules provided by the shell [ANGULARarchitects](https://www.angulararchitects.io/en/blog/multi-framework-and-version-micro-frontends-with-module-federation-your-4-steps-guide/)
- Be careful with CSS specificity and naming conflicts
- Test thoroughly with multiple MFEs loaded simultaneously
- Consider using CSS-in-JS solutions or CSS Modules for better isolation

Would you like me to provide code examples for any of these approaches?

Citations:
- [Multi-Framework and -Version Micro Frontends with Module Federation: Your 4 Steps Guide - ANGULARarchitects](https://www.angulararchitects.io/en/blog/multi-framework-and-version-micro-frontends-with-module-federation-your-4-steps-guide/)
- [angular - Serving styles and assets with Webpack 5 module federation - Stack Overflow](https://stackoverflow.com/questions/67633345/serving-styles-and-assets-with-webpack-5-module-federation)
- [CSS isolation with Angular Micro frontends | by Eugene Pavliy | Medium](https://medium.com/@epavliy/css-isolation-with-angular-micro-frontends-eb3bd8261bb9)
- [sharing common assets in angular’s module federation | It's not a bug, it's a feature](https://itsnotbugitsfeature.com/2022/03/13/sharing-common-assets-in-angulars-module-federation/)

More sources:
- [Creating Angular Micro Frontends with Native Federation](https://loiane.com/2025/01/angular-micro-frontends-with-native-federation/)
- [Transforming Your Application into Micro Frontends with Native Federation for Angular— Part 1 | by Erick Zanetti | Medium](https://medium.com/@erickzanetti/transforming-your-application-into-micro-frontends-with-native-federation-for-angular-part-1-791d159b09c8)
- [Micro Frontends with Native Federation 🍿 - DEV Community](https://dev.to/florianrappl/micro-frontends-with-native-federation-56j4)
- [Transforming Your Application into Micro Frontends with Native Federation for Angular — Part 2 | by Erick Zanetti | Medium](https://medium.com/@erickzanetti/transforming-your-application-into-micro-frontends-with-native-federation-for-angular-part-2-e8f0c60bdd63)
- [Micro-frontends with Angular - Module federation](https://module-federation.io/practice/frameworks/angular/angular-mfe)
- [Advanced Angular Micro Frontends with Dynamic Module Federation | Nx](https://nx.dev/recipes/angular/dynamic-module-federation-with-angular)
- [Micro Frontends with Angular, Module Federation | Auth0](https://auth0.com/blog/micro-frontends-with-angular-module-federation-and-auth0/)
- [Module Federation in Angular: A Step-by-Step Guide | by Sujitha Kumar | Medium](https://medium.com/@sujithakumars/module-federation-in-angular-a-step-by-step-guide-a257baefa760)
- [angular-architects/native-federation](https://www.npmjs.com/package/@angular-architects/native-federation)
- [Importing the CSS for remote components · module-federation/module-federation-examples · Discussion #714](https://github.com/module-federation/module-federation-examples/discussions/714)
- [@angular-architects/module-federation - npm](https://www.npmjs.com/package/@angular-architects/module-federation)
- [Module Federation with Angular's Standalone Components - ANGULARarchitects](https://www.angulararchitects.io/en/blog/module-federation-with-angulars-standalone-components/)
- [Native Federation: NG0203 · Issue #380 · angular-architects/module-federation-plugin](https://github.com/angular-architects/module-federation-plugin/issues/380)