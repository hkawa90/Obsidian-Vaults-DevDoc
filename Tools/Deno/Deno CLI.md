### Command line arguments
```ts title="# Command line arguments"
const name = Deno.args[0];
const food = Deno.args[1];
console.log(`Hello ${name}, I like ${food}!`);
import { parseArgs } from "jsr:@std/cli/parse-args";
const flags = parseArgs(Deno.args, {
  boolean: ["help", "color"],
  string: ["version"],
  default: { color: true },
  negatable: ["color"],
});
console.log("Wants help?", flags.help);
console.log("Version:", flags.version);
console.log("Wants color?:", flags.color);
console.log("Other:", flags._);
```

### Executing external commands

```ts
#!/usr/bin/env -S deno run --allow-all
import $ from "https://deno.land/x/dax/mod.ts";

const result = await $`deno eval 'console.log(1); console.error(2);'`
  .stdout("piped")
  .stderr("piped");
console.log(result.code); // 0
console.log(result.stdoutBytes); // Uint8Array(2) [ 49, 10 ]
console.log(result.stdout); // 1\n
console.log(result.stderr); // 2\n
const output = await $`echo '{ "test": 5 }'`.stdout("piped");
console.log(output.stdoutJson);
```

### Terminal User Interface

```ts
import { Button } from "https://deno.land/x/tui@2.1.11/src/components/mod.ts";
import { Signal, Computed } from "https://deno.land/x/tui@2.1.11/mod.ts";

...

// Create signal to make number automatically reactive
const number = new Signal(0);

const button = new Button({
  parent: tui,
  zIndex: 0,
  label: {
    text: new Computed(() => number.value.toString()), // cast number to string
  },
  theme: {
    base: crayon.bgRed,
    focused: crayon.bgLightRed,
    active: crayon.bgYellow,
  },
  rectangle: {
    column: 1,
    row: 1,
    height: 5,
    width: 10,
  },
});

  // If button is active (pressed) make number bigger by one
button.state.when("active", (state) => {
  ++number.value;
});

// Listen to mousePress event
button.on("mousePress", ({ drag, movementX, movementY }) => {
  if (!drag) return;

  // Use peek() to get signal's value when it happens outside of Signal/Computed/Effect
  const rectangle = button.rectangle.peek();
  // Move button by how much mouse has moved while dragging it
  rectangle.column += movementX;
  rectangle.row += movementY;
});
```