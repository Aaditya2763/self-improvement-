# Browser Rendering & Performance

## Learning Objectives
- Understand browser rendering pipeline
- Master critical rendering path
- Learn performance optimization
- Know browser internals

## Topics

### 1. Browser Architecture
- User interface
- Browser engine
- Rendering engine
- Networking layer
- JavaScript engine
- Data persistence

### 2. Rendering Pipeline (CRP)
1. **Parsing**: Parse HTML and CSS
2. **Compute**: Calculate styles
3. **Layout**: Arrange elements
4. **Paint**: Draw pixels
5. **Composite**: Combine layers

### 3. DOM and CSSOM
- DOM: Document Object Model
- CSSOM: CSS Object Model
- Render tree: Combination of DOM + CSSOM
- Only visible elements in render tree

### 4. JavaScript and Rendering
- Script parsing blocks HTML
- defer: Execute after parsing
- async: Execute asynchronously
- Document.write() blocks rendering

### 5. Performance Metrics
- FCP: First Contentful Paint
- LCP: Largest Contentful Paint
- TTI: Time to Interactive
- CLS: Cumulative Layout Shift
- FID: First Input Delay

## Critical Rendering Path

```
HTML → Parse DOM → CSSOM ┐
                         → Render Tree → Layout → Paint → Composite
CSS  ─────────────────────┘

JavaScript can:
- Query DOM/CSSOM
- Modify DOM/CSSOM
- Trigger reflow/repaint
```

## Optimization Strategies

### Load Optimization
```html
<!-- Critical styles inline -->
<style>
  body { color: black; }
</style>

<!-- Defer non-critical CSS -->
<link rel="stylesheet" href="style.css" media="print">

<!-- Script optimization -->
<script defer src="app.js"></script>
```

### Performance Tips
1. Minimize main thread work
2. Reduce JavaScript execution time
3. Use CSS animations instead of JS
4. Defer non-critical JavaScript
5. Use web workers for heavy tasks
6. Cache resources properly
7. Compress images
8. Use CDN

## Reflow vs Repaint
- **Reflow**: Recalculate layout (expensive)
- **Repaint**: Redraw pixels without layout change
- **Composite**: Combine layers (GPU accelerated)

## DevTools Profiling
1. Open Chrome DevTools
2. Go to Performance tab
3. Start recording
4. Perform actions
5. Stop recording
6. Analyze results

## Practice Tasks
- [ ] Profile web pages
- [ ] Identify bottlenecks
- [ ] Optimize rendering
- [ ] Use performance APIs
- [ ] Measure Core Web Vitals

## Interview Tips
- Explain rendering pipeline
- Know CRP optimization
- Understand reflow/repaint
- Know performance metrics
- Discuss optimization strategies

## Performance APIs
```javascript
// Measure timing
performance.timing
performance.navigation

// Mark and measure
performance.mark('operation-start');
// ... operation ...
performance.mark('operation-end');
performance.measure('operation', 'operation-start', 'operation-end');

// Get metrics
const paintEntries = performance.getEntriesByType('paint');
```

## Resources
- Google Developers: CRP
- MDN: Critical Rendering Path
- WebPageTest
- Google PageSpeed Insights
