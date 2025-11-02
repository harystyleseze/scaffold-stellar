### Step-by-Step Guide Using Scaffold Stellar

#### Phase 1: Installation & Setup

1.  **Install the Scaffold Stellar CLI:** This is your main tool. Open your terminal and run:
    ```bash
    cargo install --locked stellar-scaffold-cli
    ```
    *This installs the `stellar` command with the `scaffold` plugin.*

2.  **Prerequisites:** Ensure you have the basics from the previous guide:
    *   **Rust & Cargo:** `rustc --version`
    *   **Node.js & npm:** `node --version`
    *   **Freighter Wallet:** Installed in your browser and set to **Testnet**.

#### Phase 2: Create Your Project

This single command replaces *all* the manual project creation and setup.

1.  **Initialize Your Project:** Navigate to the folder where you want to create your project and run:
    ```bash
    stellar scaffold init rent-a-car-dapp
    cd rent-a-car-dapp
    ```
    This command does the magic:
    *   Creates a new directory `rent-a-car-dapp` with the perfect folder structure.
    *   Sets up a **React + TypeScript frontend** with Vite (a fast build tool).
    *   Pre-installs all necessary dependencies (`@stellar/stellar-sdk`, `@creit.tech/stellar-wallets-kit`, etc.).
    *   Creates example smart contracts and their TypeScript clients.

2.  **Install Frontend Dependencies:** The CLI sets up the package.json, but you need to install the modules.
    ```bash
    npm install
    ```

#### Phase 3: Write and Deploy Your Smart Contract

Now, you don't need to manually create the contract project. Scaffold Stellar has already created a `contracts/` folder for you.

1.  **Locate the Contract Code:** Navigate to `rent-a-car-dapp/contracts/`. You'll likely see an example contract (e.g., `hello_world` or `increment`).
2.  **Create Your Contract:**
    *   You can create a new contract folder, e.g., `rent_a_car`, or replace the code in an existing example.
    *   **The key file is `src/lib.rs`.** Replace its contents with the full `lib.rs` code from the workshop.
3.  **Build the Contract:** From the project's **root directory**, you can build your contract. The scaffold tools make this easier.
    ```bash
    # This builds all contracts in the /contracts folder
    stellar scaffold build
    ```
    *The optimized `.wasm` file will be placed in `target/stellar/local/`.*

4.  **Deploy to Testnet using the Registry (The Scaffold Way):** This is the biggest advantage. Instead of complex CLI commands, you use the built-in registry system.
    ```bash
    # 1. Publish your contract WASM to the registry
    stellar registry publish --wasm target/stellar/local/rent_a_car.wasm --wasm-name rent-a-car

    # 2. Deploy an instance of your contract, passing the constructor arguments
    stellar registry deploy \
      --contract-name rent-a-car-instance \
      --wasm-name rent-a-car \
      -- \
      --admin <YOUR_FREIGHTER_PUBLIC_KEY> \
      --token CDLZFC3SYJYDZT7K67VZ75HPJVIEUVNIXF47ZG2FB2RMQQVU2HHGCYSC

    # 3. Install it locally so your frontend can find it
    stellar registry install rent-a-car-instance
    ```
    After running this, your contract is live on Testnet, and its ID is stored locally under the name `rent-a-car-instance`.

#### Phase 4: Configure the Frontend

1.  **Set the Contract ID:** The `stellar registry install` command likely already updated your configuration. But you should check the environment files.
    *   Open `.env` in the root of your project.
    *   Ensure it points to the testnet and that the contract name is correctly referenced. The scaffold often uses a `.env` variable like `VITE_CONTRACT_NAME=rent-a-car-instance`.

2.  **Rebuild the TypeScript Client:** After deploying, you need to update the auto-generated code that lets your frontend talk to the contract.
    ```bash
    stellar scaffold build
    ```
    This command regenerates the TypeScript client in the `packages/` folder based on your deployed contract.

#### Phase 5: Build the Frontend UI

This part remains largely the same as the manual process, but the structure is already created for you.

1.  **Navigate to the `src/` folder.** You'll see a pre-built structure with `App.tsx`, `components/`, etc.
2.  **Replace the contents** of the auto-generated files with the code from the workshop. Start with:
    *   `src/App.tsx`
    *   `src/utils/constants.ts` (make sure the constants point to your testnet settings and contract name)
    *   Then create the `components/`, `pages/`, `services/`, and `interfaces/` folders as shown in the workshop, adding the files one by one.
3.  **Run the Development Server:**
    ```bash
    npm run dev
    ```
    Your dApp will open at `http://localhost:5173`. Connect your Freighter wallet and test it!

---

### Why This is Easier with Scaffold Stellar:

| Manual Workshop Step | With Scaffold Stellar |
| :--- | :--- |
| Run multiple `cargo` and `soroban` commands | Use `stellar scaffold build` and `stellar registry` commands |
| Manually create React project and install deps | `stellar scaffold init` does it all instantly |
| Manually copy contract ID into frontend config | `stellar registry install` manages this automatically |
| Manually write TypeScript client interfaces | `stellar scaffold build` auto-generates them from your Rust code |
| Complex deployment command | Simplified `registry publish` and `registry deploy` workflow |
. The Scaffold Stellar kit removes the biggest barriers to entry. Now you can focus on the fun part: building your dApp. Good luck
