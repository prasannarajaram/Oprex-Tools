# Oprex-Tools
Some tools to make life easier with Oprex forms
1. [Expand all sections in an Oprex page](https://github.com/prasannarajaram/Oprex-Tools/blob/main/README.md#expand-all-sections-in-an-oprex-page)
2. [Select all checkboxes](https://github.com/prasannarajaram/Oprex-Tools/blob/main/README.md#select-all-checkboxes)
3. [Copy review 1 to review 2](https://github.com/prasannarajaram/Oprex-Tools/blob/main/README.md#copy-review-1-to-review-2)
4. [Sign Reviews](https://github.com/prasannarajaram/Oprex-Tools/blob/main/README.md#sign-reviews)

## Expand all sections in an Oprex page
Many a times it is required to search the filled in Oprex form or a specific Oprex question / answer.  
The search (`Cntl+F`) does not return results of the particular section if is not visible (or expanded)  
So, the user is forced to click the expand buttons multiple times to reveal all text to make a simple search.  
To mitage this problem, we have the JS function below, which will expand all the sections of an Oprex page.  
To create this button, go to bookmarks (favorites) in the browser and create a new one.  
Instead of providing a url, copy paste the below JS function into URL box. Save.  
```javascript
javascript:(function() {    const buttons = document.querySelectorAll('span[id^="expand-arrow"]');    buttons.forEach(btn => btn.click());})();
```
### Here is short video
https://github.com/user-attachments/assets/b1da3ab9-2b06-446d-8efb-2853d0027647

## Select all checkboxes
When creating an "Audit view" (read as exporting Oprex review to PDF), there are four check boxes that the user must select to export the complete Oprex form (with review) as PDF.  
It is possible that one of the checkboxes could be missed and the resulting PDF may not have the complete export of the Oprex for review.  
To make this easier, we have the JS function below which will "check" all the checkboxes in given Oprex form.  
To create this button, go to bookmarks (favorites) in the browser and create a new one.    
Instead of providing a url, copy paste the below JS function into URL box. Save.  

_Pitfalls: Clicking it twice will uncheck everything. Worst, it will toggle selection if one the checkbox is selected_
```javascript
javascript:(function() {    document.querySelectorAll('input[type="checkbox"]').forEach(cb => cb.click());    })();
```
## Copy review 1 to review 2
Many a times, it is required to duplicate the responses entered in one of the reviews to another review. For instance, GDR of similar type of cabinets will require the same remarks for same type of cabinets (of course with minor modification). In order to avoid copy-paste errors, the following JS code (bookmarklet) can be used to copy the reviews from one to another.
```javascript
javascript:(()=>{document.querySelectorAll('.MuiGrid-root.MuiGrid-item').forEach(e=>{const t=e.querySelectorAll('textarea'),r=Array.from(t).find(e=>e.hasAttribute('readonly')&&!e.hasAttribute('aria-hidden')),a=Array.from(t).find(e=>!e.hasAttribute('readonly')&&!e.disabled);if(r&&a){const e=r.value;Object.getOwnPropertyDescriptor(window.HTMLTextAreaElement.prototype,'value').set.call(a,e),a.dispatchEvent(new Event('input',{bubbles:!0})),a.dispatchEvent(new Event('blur',{bubbles:!0}))}});})();
```
## Sign Reviews
### _This is not a bookmarklet, it is a JS function to be run in console_
When the sign button is clicked in Oprex, the review is signed; the section folds; the user has to click the expand button to go to the next review to sign.
If the number of reviews are more, then this can easily take up to 1 min/review. This JS function will click the sign button for the user.
The code has a built in delay of 30s after clicking sign and 30s delay to allow the expansion of section.

### [A Short Video](https://youtu.be/CYXOi9ZyLOU)
```javascript
(async function runSignRoutineWithCount() {
  function wait(ms) { return new Promise(res => setTimeout(res, ms)); }

  function isVisible(el) {
    if (!el) return false;
    const s = window.getComputedStyle(el);
    if (s.display === "none" || s.visibility === "hidden" || parseFloat(s.opacity) === 0) return false;
    const r = el.getBoundingClientRect();
    return r.width > 0 && r.height > 0;
  }

  function findSignButton() {
    // Prefer visible elements with id="drc-sign-review"
    const byId = [...document.querySelectorAll('#drc-sign-review')].filter(isVisible);
    if (byId.length) return byId[0];

    // Fallback: any visible <button> whose visible text is "SIGN"
    const byText = [...document.querySelectorAll('button')].filter(b => {
      const t = (b.innerText || "").trim().toUpperCase();
      return t === "SIGN" && isVisible(b);
    });
    if (byText.length) return byText[0];

    return null;
  }

  async function expandLastAccordion() {
    const nodes = [...document.querySelectorAll('[id^="drc-icon-expand-accordion"], [id^="expand-arrow"]')];
    if (!nodes.length) {
      console.log("⚠️ No accordion buttons found.");
      return false;
    }
    const last = nodes[nodes.length - 1];
    try {
      last.click();
      console.log("✅ Clicked last accordion toggle.");
      return true;
    } catch (err) {
      console.warn("⚠️ Failed to click last accordion:", err);
      return false;
    }
  }

  // Count sign buttons at the start
  const initialSignButtons = document.querySelectorAll('#drc-sign-review');
  const totalAttempts = initialSignButtons.length;
  console.log(`🔢 Found ${totalAttempts} sign button(s). Will attempt ${totalAttempts} time(s).`);

  if (totalAttempts === 0) {
    console.log("ℹ️ No SIGN buttons found. Exiting.");
    return;
  }

  // allow external cancellation: set window._stopSignRoutine = true from console to stop
  window._stopSignRoutine = false;

  for (let i = 0; i < totalAttempts; i++) {
    if (window._stopSignRoutine) {
      console.log("🛑 Routine cancelled by user.");
      break;
    }

    const iteration = i + 1;
    const signBtn = findSignButton();
    if (!signBtn) {
      console.warn(`⚠️ SIGN button not found at iteration ${iteration}. Exiting early.`);
      break;
    }

    try {
      signBtn.click();
      console.log(`🔘 Clicked SIGN (${iteration}/${totalAttempts})`);
    } catch (err) {
      console.warn(`⚠️ Failed to click SIGN at iteration ${iteration}:`, err);
    }

    // wait 30s after clicking SIGN
    await wait(30000);

    // expand only the last accordion
    console.log("📂 Expanding last accordion...");
    await expandLastAccordion();

    // wait 30s after expansion
    await wait(30000);
  }

  console.log("✅ Routine finished.");
})();
```
