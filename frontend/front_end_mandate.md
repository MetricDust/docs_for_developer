# Rules for frontend developers which should be followed Mandatorily

## Front end Coding practices

-   Code must be **logs free** before being deployed or committed.
-   Code must not have **unwanted commented function or variables**. But can have comments for documentation purpose.
-   Function must follow a particular flow in **hierarchical manner**. Like first function which lodes when page is loaded must come first and others must be one below other in the hierarchy.
-   Names of variable and function must be self explanatory and with proper formate either in **camelCase** or **snake_case** and it must be followed through the code.
-   Variable must be assigned with **proper type** of it like string, boolean, number etc.
-   Proper **comments** must be used for functions parameters and return type when ever needed or the name for the **params can be self explanatory**.
-   Every **if** condition must be followed with **else**. when there is nothing in else block it can be have just comments
-   **deprecated syntax** must be avoided and if any must be changed to proper syntax.
-   Class and styles must be maintained as mush as in generic way and those can be kept in **styles.scss** file.
-   Try to make the page **stateless** as musch as possible using queryparams or path params. **preferred query params**
-   Where ever the id or any other params is required is taken form the **query params it must be single variable** and must be using same variable when needed.
-   Use of observables can be reduces and **signals** can be used as alternative.
-   Local storage must be **secured or encrypted** and must clear them on logout or closed.
-   **git ignore** must be containing unwanted files which is not required in the cloud like node_modules, logs, vscode settings, etc.
-   Services must be maintained for api calls and generic functions. generic function can be added in `common.service.ts` file and respective services file can be made for other use cases.
-   Its best to maintain **2 levels of parent child hierarchy in components**, at max 3 levels not more. And if its 3 levels the api call or control of data must be always maintained in the parent or the last child not in the middle.

## The pattern of the component ts file must be followed as bellow.

```typescript
import { Component } from "@angular/core";
import { FormBuilder, FormGroup, Validators } from "@angular/forms";
// other imports will be here

@Component({
    selector: "app-sample",
    templateUrl: "./sample.component.html",
    styleUrls: ["./sample.component.css"],
})
export class SampleComponent {
    // all the injectable will be at first
    _service = inject(SampleService);

    // sample variables will be second
    sample_variables: type;

    // constructor will be third
    constructor(private fb: FormBuilder) {}

    // ngOnInit must be forth
    ngOnInit() {
        // respective code

        // function must be called in hierarchical manner
        this.function_1();
        this.function_2();
    }

    // onChanges function must be fifth
    onChanges() {
        // respective code
    }

    // reset function must be sixth if its required
    reset() {
        // respective code
    }

    function_1() {
        // respective code
    }

    function_2() {
        // respective code
        this.function_3();
    }

    function_3() {
        // respective code
    }

    // submit function must be the last one before generic function
    submit() {}

    // last must be the generic function with comment as bellow
    // generic function
    function_4() {
        return something;
    }
}
```

This pattern will help in code review and debugging.

### Command to change CRLF to LF in bash scripts

```bash
sed -i 's/\r//' deploy_to_beta.sh
sed -i 's/\r//' deploy_to_prod.sh
sed -i 's/\r//' deploy.sh
```
