<script>
  let strength = 0;
  let validations = [];
  let showPassword = false;

  function validatePassword(e) {
    const password = e.target.value;
    validations = [
      password.length > 5,
      password.search(/[A-Z]/) > -1,
      password.search(/[0-9]/) > -1,
      password.search(/[$&+,:;=?@#]/) > -1,
    ];
    strength = validations.reduce((acc, cur) => acc + cur);
  }
</script>

<style>
  form {
    --text-color: #afafaf;
    max-width: 500px;
  }

  .field {
    width: 100%;
    position: relative; /* ALLOWS YOU TO USE top, left, rigth and bottom attributes */
    border-bottom: 2px dashed var(--text-color);
    margin: 4rem auto 1rem;
  }

  .input {
    outline: none;
    border: none;
    overflow: hidden;
    margin: 0;
    width: 100%;
    padding: 0.25rem 0;
    background: none;
    color: white;
    font-size: 1.2rem;
    font-weight: bold;
  }

  .input:valid {
    color: yellowgreen;
  }

  .input:invalid {
    color: orangered;
  }

  /* BORDER ANIMATION */

  .field::after {
    content: "";
    position: relative;
    display: block;
    height: 4px;
    width: 100%;
    background-color: #d16dff;
    transform: scaleX(0);
    transform-origin: 0%; /* so it will animate from left to rigth */
    transition: transform 500ms ease;
    top: 2px;
  }

  .field:focus-within {
    /* ":focus-within" ACTIVE WHEN CHILD HAS FOCUS */
    border-color: transparent;
  }

  .field:focus-within::after {
    transform: scaleX(1);
  }

  /* LABEL ANIMATION */
  .label {
    z-index: -1;
    position: absolute;
    transform: translateY(-2rem);
    transform-origin: 0%;
    transition: transform 400ms;
  }

  .field:focus-within .label,
  .input:not(:placeholder-shown) + .label {
    /* "+ .form--label" targets the next sibling with the class 'form--label' */
    transform: scale(0.8) translateY(-5rem);
  }

  /* PASSWORD STRENGTH */
  .strength {
    display: flex;
    height: 20px;
    width: 100%;
  }

  .bar {
    margin-right: 5px;
    height: 100%;
    width: 25%;

    transition: box-shadow 500ms;
    box-shadow: inset 0px 20px #1f1f1f;
  }

  .bar-show {
    box-shadow: none;
  }

  .bar-1 {
    background: linear-gradient(to rigth, red, orangered);
  }

  .bar-2 {
    background: linear-gradient(to rigth, orangered, yellow);
  }

  .bar-3 {
    background: linear-gradient(to rigth, yellow, yellowgreen);
  }

  .bar-4 {
    background: linear-gradient(to rigth, yellowgreen, green);
  }

  .toggle-password {
    position: absolute;
    cursor: help;
    font-size: 0.8rem;
    right: 0.25rem;
    bottom: 0.5rem;
  }
</style>

<main>
  <form action="">
    <div class="field">
      <input type="email" name="email" class="input" placeholder="" />
      <label for="email" class="label">Email</label>
    </div>
    <div class="field">
      <input
        type={showPassword ? 'text' : 'password'}
        name="password"
        class="input"
        placeholder=""
        on:input={validatePassword} />
      <label for="password" class="label">Password</label>

      <span
        class="toggle-password"
        on:mouseenter={() => (showPassword = true)}
        on:mouseenter={() => (showPassword = false)}>
        {showPassword ? 'O.O' : '-.-'}
      </span>
    </div>

    <div class="strength">
      <span class="bar bar-1" class:bar-show={strength > 0} />
      <span class="bar bar-2" class:bar-show={strength > 1} />
      <span class="bar bar-3" class:bar-show={strength > 2} />
      <span class="bar bar-4" class:bar-show={strength > 3} />
    </div>
    <ul>
      <li>must be at leat 5 characters long</li>
      <li>must contain a capital letter</li>
      <li>must contain a number</li>
      <li>must contain one of '$&+,:;=?@#'</li>
    </ul>
  </form>
</main>
