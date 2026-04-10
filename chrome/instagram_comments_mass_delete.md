# Instagram Comments Mass Delete

### FIRST manually click "Select" on the page, THEN run this script

```
setTimeout(() => {
  const checkboxes = [...document.querySelectorAll('div[aria-label="Toggle checkbox"]')];
  console.log(`Found ${checkboxes.length} checkboxes`);

  if (checkboxes.length === 0) {
    console.warn('No checkboxes found - make sure you clicked Select first!');
    return;
  }

  checkboxes.forEach((cb, i) => {
    setTimeout(() => {
      // Bypass pointer-events: none by dispatching events on the parent
      const target = cb.parentElement || cb;
      target.dispatchEvent(new PointerEvent('pointerdown', { bubbles: true, cancelable: true }));
      target.dispatchEvent(new PointerEvent('pointerup', { bubbles: true, cancelable: true }));
      target.dispatchEvent(new MouseEvent('click', { bubbles: true, cancelable: true }));
      cb.dispatchEvent(new MouseEvent('click', { bubbles: true, cancelable: true }));
      console.log(`✅ Selected ${i + 1}/${checkboxes.length}`);
    }, i * 150);
  });

  setTimeout(() => {
    const count = document.querySelector('span');
    console.log('👆 Done selecting! Now click the Delete button.');
  }, checkboxes.length * 150 + 500);
}, 500);
```
