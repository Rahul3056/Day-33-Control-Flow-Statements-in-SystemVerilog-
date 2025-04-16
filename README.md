### **Day 33: Control Flow Statements in SystemVerilog**


#### **1. Introduction**

Control flow statements are used to alter the sequence of execution in procedural blocks (e.g., `initial`, `always`, `always_ff`, and within functions/tasks). These are fundamental constructs used in both design and testbench code.

SystemVerilog supports all the control flow constructs found in traditional programming languages like C/C++, and also allows hardware-specific behavior modeling.

---

#### **2. Types of Control Flow Statements**

The major control flow constructs in SystemVerilog include:

1. **Conditional Statements**
2. **Looping Constructs**
3. **Branching Statements**
4. **Case Statements**

---

#### **3. Conditional Statements**

##### `if`, `else if`, `else`

Used to make decisions based on Boolean conditions.

**Syntax:**

```systemverilog
if (condition)
    statement;
else if (condition2)
    statement2;
else
    default_statement;
```

**Example:**

```systemverilog
if (a == 1)
    y = 0;
else if (a == 2)
    y = 1;
else
    y = 'z;  // high impedance
```

---

#### **4. Looping Constructs**

Used for repeating a set of operations.

##### a) `for` loop

Useful for fixed or counted iteration.

```systemverilog
for (int i = 0; i < 10; i++) begin
    $display("i = %0d", i);
end
```

##### b) `while` loop

Runs as long as the condition is true.

```systemverilog
int i = 0;
while (i < 5) begin
    $display("i = %0d", i);
    i++;
end
```

##### c) `do...while` loop

Executes at least once before checking the condition.

```systemverilog
int i = 0;
do begin
    $display("i = %0d", i);
    i++;
end while (i < 3);
```

##### d) `foreach` loop

Specifically for arrays and queues. SystemVerilog extension.

```systemverilog
int arr[3] = '{1, 2, 3};
foreach (arr[i])
    $display("arr[%0d] = %0d", i, arr[i]);
```

---

#### **5. Case Statements**

Used when multiple conditions depend on a single variable.

##### a) `case` (4-state)

```systemverilog
case (sel)
    2'b00: y = a;
    2'b01: y = b;
    default: y = 'z;
endcase
```

##### b) `unique case`

Ensures only one match. If more than one matches, simulation error.

```systemverilog
unique case (sel)
    2'b00: y = a;
    2'b01: y = b;
    default: y = c;
endcase
```

##### c) `priority case`

First matching condition is selected; later matches are ignored.

---

#### **6. Branching Statements**

Used to modify the control flow inside loops.

##### a) `break`

Exits from the innermost loop.

```systemverilog
for (int i = 0; i < 10; i++) begin
    if (i == 5) break;
    $display("i = %0d", i);
end
```

##### b) `continue`

Skips the current iteration and continues with the next one.

```systemverilog
for (int i = 0; i < 10; i++) begin
    if (i == 5) continue;
    $display("i = %0d", i);
end
```

---

#### **7. Differences from Verilog**

| Feature         | Verilog | SystemVerilog |
|----------------|---------|----------------|
| `foreach` loop | Not supported | Supported |
| `break`, `continue` | Not supported | Supported |
| `do...while` | Not supported | Supported |
| `unique`, `priority` case | Not available | Supported |
| Complex conditions with arrays | Manual | Simplified with `foreach` |

---

#### **8. Use in Testbenches vs Design**

- In **testbenches**, loops and conditionals are used freely for generating stimuli, checking outputs, and driving sequences.
- In **RTL design**, all control flow must be synthesizable. Avoid infinite or unpredictable loops (`while(1)` without a break) and be cautious with complex `if` conditions that lead to latches.

---

#### **9. Common Pitfalls**

- Omitting `begin...end` in multi-statement blocks
- Using non-synthesizable constructs in design code (e.g., `break`, `continue`)
- Not covering all cases in a `case` statement, leading to latches
- Unintentional latch inference when `if` or `case` doesn’t assign all outputs

---

#### **10. Best Practices**

- Always use `default` in `case` statements to avoid unintended latches
- Prefer `foreach` over manual indexing for array iterations
- In testbenches, use `break` and `continue` to manage loops clearly
- Use `unique` or `priority` in `case` statements for clarity and coverage checking
