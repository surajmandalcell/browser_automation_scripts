# Instagram Comments Mass Delete

### FIRST manually click "Select" on the page, THEN run this script

Ideal is about 25-35 per try, take it slow since most of the times you'll get rate limited. 

```
const select_no = 25;

async function run() {
  // Click "Select" button if checkboxes aren't visible yet
  const checkboxes = () => [...document.querySelectorAll('div[aria-label="Toggle checkbox"]')];

  if (checkboxes().length === 0) {
    const selectBtn = [...document.querySelectorAll('*')].find(
      el => el.textContent.trim() === 'Select' && el.role !== undefined || 
            el.getAttribute?.('aria-label') === 'Select'
    ) || [...document.querySelectorAll('div, span, button')].find(
      el => el.textContent.trim() === 'Select' && !el.children.length
    );

    if (!selectBtn) {
      console.warn('Could not find Select button!');
      return;
    }

    console.log('Clicking Select button...');
    selectBtn.click();
    await new Promise(r => setTimeout(r, 800));
  }

  const boxes = checkboxes();
  console.log(`Found ${boxes.length} checkboxes`);

  if (boxes.length === 0) {
    console.warn('Still no checkboxes found after clicking Select!');
    return;
  }

  const toSelect = boxes.slice(0, select_no);

  for (let i = 0; i < toSelect.length; i++) {
    await new Promise(r => setTimeout(r, 150));
    const cb = toSelect[i];
    const target = cb.parentElement || cb;
    target.dispatchEvent(new PointerEvent('pointerdown', { bubbles: true, cancelable: true }));
    target.dispatchEvent(new PointerEvent('pointerup', { bubbles: true, cancelable: true }));
    target.dispatchEvent(new MouseEvent('click', { bubbles: true, cancelable: true }));
    cb.dispatchEvent(new MouseEvent('click', { bubbles: true, cancelable: true }));
    console.log(`Selected ${i + 1}/${toSelect.length}`);
  }

  console.log('Done selecting! Now click the Delete button.');
}

run();
```
