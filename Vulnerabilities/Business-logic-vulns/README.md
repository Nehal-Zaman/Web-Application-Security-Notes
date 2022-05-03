# Business logic vulns

Business logic vulnerabilities are flaws in the design and implementation of an application that allows an attacker to cause an unintended behavior.

These sort of vulnerabilities are specific to a particular application. They arise when the design and development team make flawed assumption about how the user will interact with the application. These bad assumptions eventually leads to inadequate validation of user input.

The impact of business logic vulnerabilities depends on the functionality of the application. If the logic flaw is in authentication functions, for example, the impact is quite high.

## Examples of business logic vulns:

- **Excessive trust in client-side controls**: A dangerous flaw is that the developers assume that the user will interact with the application only through the web interface, and hence they implement some client side validation thinking that the user will not be able to control these. However, these client side controls can easily be bypassed using proxy tools like `Burpsuite`.
- **Failing to handle unconventional input:** The application logic is aimed to restrict user input to values that adhere to the business rules. If the unconventional user inputs are not handled the right way, it can give rise to logic flaws that can have devastating impact if they fit in the right functionality. By observing the application responses, we should try and answer the below questions:
    - Are there any limits that are imposed on the data ?
    - What happens when we reach those limits ?
    - Is any transformation or normalization being performed in our input ?
    - E.g. → High level logic flaws, low level logic flaws or inconsistent handling of user input.
- **Making flawed assumptions about user behavior**:
    - **Trusted users won’t always remain trustworthy**: Some applications make an assumption that after passing some security controls initially, the user and its data can be trusted indefinitely. Later this may lead to potentially dangerous consequence.
    - **Users don’t always supply mandatory input**: Some application may expect some mandatory input from user in the input fields. Since this mandatory check is being done by the browser in the client side, it can easily be bypassed. When testing for logic flaws you should try removing/changing the value of each parameter and observe what effect it has on the response.
    - **Users won’t always follow the intended sequence**: Some applications rely on a workflow that uses a sequence of steps that will be guided by the web interface. Attackers won’t necessarily adhere to this sequence, which may lead to bypass something entirely. Take a note of every step in the flow. Sometimes application may give some error message if you jump to some other step. The error message can be a vital source of information disclosure that may be used to understand the back-end technology.
- **Domain specific flaws**: You should look carefully how sensitive data are manipulated through user actions. Try to think of the algorithm that might be designed for the same. Play with it to see if you can find any bug.
- **Providing an encryption oracle**: Dangerous scenarios can occur when user-controllable input is encrypted and the resulting ciphertext is then made available to the user in some way. This kind of input is sometimes known as an "encryption oracle". An attacker can use this input to encrypt arbitrary data using the correct algorithm and asymmetric key.