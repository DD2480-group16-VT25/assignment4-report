# Report for assignment 4

## Project

Name: Material UI

URL: https://github.com/mui/material-ui/

Material UI is a comprehensive library of React components that features our independent implementation of Google's Material Design system. (from their GitHub README)

## Onboarding experience

Did you choose a new project or continue on the previous one? We chose a new project

If you changed the project, how did your experience differ from before?

## Effort spent

For each team member, how much time was spent in

1. plenary discussions/meetings;

2. discussions within parts of the group;

3. reading documentation;

4. configuration and setup;

5. analyzing code/output;

6. writing documentation;

7. writing code;

8. running code?

For setting up tools and libraries (step 4), enumerate all dependencies
you took care of and where you spent your time, if that time exceeds
30 minutes.

## Overview of issue(s) and work done.

Title:

URL:

Summary in one or two sentences

Scope (functionality and code affected).

## Requirements for the new feature or requirements affected by functionality being refactored

Optional (point 3): trace tests to requirements.

## Code changes

### Patch

```diff
diff --git a/packages/mui-material/src/utils/useSlot.ts b/packages/mui-material/src/utils/useSlot.ts
index 90f9ffc4bd..6c8dfd97dc 100644
--- a/packages/mui-material/src/utils/useSlot.ts
+++ b/packages/mui-material/src/utils/useSlot.ts
@@ -135,6 +135,11 @@ export default function useSlot<
     {
       ...(name === 'root' && !rootComponent && !slots[name] && internalForwardedProps),
       ...(name !== 'root' && !slots[name] && internalForwardedProps),
+      ...((name === 'input' || elementType === 'input' || name.includes('input')) &&
+        'disabled' in ownerState &&
+        ownerState.disabled !== undefined && {
+          'aria-disabled': !!ownerState.disabled || undefined,
+        }),
       ...mergedProps,
       ...(LeafComponent &&
         !shouldForwardComponentProp && {
```

Optional (point 4): the patch is clean.

Optional (point 5): considered for acceptance (passes all automated checks).

## Test results

>The test being ran are `/packages/mui-material/src/Switch/Switch.test.js`

Before touching the forked-repo:
```bash
158 passing (841ms)
```

Git diff for two new test cases:
```diff
diff --git a/packages/mui-material/src/Switch/Switch.test.js b/packages/mui-material/src/Switch/Switch.test.js
index 3f62c576b8..e986850094 100644
--- a/packages/mui-material/src/Switch/Switch.test.js
+++ b/packages/mui-material/src/Switch/Switch.test.js
@@ -159,4 +159,18 @@ describe('<Switch />', () => {
       expect(container.firstChild).to.have.class('test-class-name');
     });
   });
+
+  describe('accessibility', () => {
+    it('has attribute aria-disabled="true" when disabled', () => {
+      const { getByRole } = render(<Switch disabled />);
+
+      expect(getByRole('checkbox')).to.have.attribute('aria-disabled', 'true');
+    });
+
+    it('does not have aria-disabled when not disabled', () => {
+      const { getByRole } = render(<Switch />);
+
+      expect(getByRole('checkbox')).to.not.have.attribute('aria-disabled');
+    });
+  });
 });
```

The former test case fails before the patch:
```bash
159 passing (790ms)
12 pending
1 failing
1) <Switch />
      accessibility
        has attribute aria-disabled="true" when disabled:
    AssertionError: expected input.PrivateSwitchBase-input.MuiSwitch-input.emotion-client-render-j8yymo[disabled][type="checkbox"] to have an attribute 'aria-disabled'
    at Context.<anonymous> (packages\mui-material\src\Switch\/Switch.test.js:167:45)
    at processImmediate (node:internal/timers:491:21)
```

After the patch:
```bash
160 passing (556ms)
```

## UML class diagram and its description

### Key changes/classes affected

Optional (point 1): Architectural overview.

Optional (point 2): relation to design pattern(s).

## Overall experience

What are your main take-aways from this project? What did you learn?

How did you grow as a team, using the Essence standard to evaluate yourself?

Optional (point 6): How would you put your work in context with best software engineering practice?

Optional (point 7): Is there something special you want to mention here?
