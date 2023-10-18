[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- PROJECT LOGO -->
<br />
<div align="center"> 
  <img src="./public/tonote-logo.png" alt="Logo" width="300">

  <h3 align="center">Skoutwatch</h3>

  <p align="center">
    Football training, vetting and Recruitment
    <br />
    <a href="#"><strong>See Live Version »</strong></a>
    <br />
    <br />
    <a href="#">View Website</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
  <p>Table of Contents</p>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li> 
        <li><a href="#user-stories">User Stories</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a>
    <ul>
       <li><a href="#api-consumption">API Consumption</a></li>
    </ul>
    </li> 
  </ol>

<!-- ABOUT THE PROJECT -->

## About The Project

![Skoutwatch Backend Application](/screenshots/dashboard-view-document.png)

This is the application built with PHP that creates the endpoints of the application indepth

It takes care of:

- User Authentication
- User Country verification (Nigeria for now)
- Nft Storage of players and Attributes
- Initiating Solana Wallet

Hence, requirements 1, 2, 3 and 5 were met.

## Built With

- [![PHP][PHP]][php-url]

<!-- GETTING STARTED -->

## Getting Started

1. Clone this repository.
2. Install required dependencies using `composer install`.
3. Create a `.env` file from `.env.example` and configure database settings.
4. Run migrations using `php artisan migrate`.
5. Start the development server using `php artisan serve`.

### Prerequisites

To deploy SkoutWatch, follow these steps:

1. Set up a production server with PHP, Composer, and a web server (e.g., Nginx).
2. Configure your web server to point to the `public` directory.
3. Set up your production database in the `.env` file.
4. Install required dependencies using `composer install --no-dev`.
5. Run migrations using `php artisan migrate`.
6. Generate a unique application key using `php artisan key:generate`.
7. Configure any necessary environment-specific settings in `.env`.
8. Point your domain to the server's IP address.
9. Secure your application by following Laravel's security best practices.
10. Launch your application and start the athlete discovery revolution!
  ``



## Usage

The following shows the functionality of the application with respect to the endpoints provided.

### API Consumption

```js
// ./src/utils/constants.ts
export const API_URL = "https://dev-api.skoutwatch.com/api/v1/";
```

### 1. User Authentication

<h4 style="font-size: 1.4rem;">i. Logging in</h4>

```sh
POST /api/v1/user/login
app/Http/Controllers/Api/Auth/UserController.php
```

<table>
<tr>
<th>API Call</th>
</tr>
<tr>
<td>
  
<p>app/Http/Controllers/Api/Auth/UserController.php</p> 
<p><strong>Login Feature</strong></p>
</td>
<td>

```php
public function login(UserLoginFormRequest $request)
{
    $user = User::where('email', $request['email'])->firstOrFail();

    if (! $token = Auth::attempt(['email' => strtolower($request['email']), 'password' => $request['password']])) {
        return $this->authErrorResponse('Incorrect email or password', 401);
    }

    $user = User::find(auth('api')->user()->id);

    $user->update([
        'last_login_activity' => Carbon::now()->format('Y-m-d H:i:s'),
        'ip_address' => $request->ip(),
    ]);

    return $this->respondWithToken($user->createToken('MyApp')->plainTextToken);
}
```

</td>
</tr>
</table>

<br/>

<h4 style="font-size: 1.4rem;">ii. Registering User</h4>

```sh
POST https://dev-api.gettonote.com/api/v1/user/register
```

<table>
<tr>
<th>API Call</th>
</tr>
<tr>
<td>
  
<img src="./screenshots/register.png" alt="Logo" width="600">
<p>app/Http/Controllers/Api/Auth/UserController.php</p> 
<p><strong>Register Feature</strong></p>
</td>
<td>

```php
public function register(UserRegistrationFormRequest $request)
{
    $user = User::create(($request->validated()));

    // event(new UserRegistered($user));

    // (new SolanaWalletService($user))->createUserWallet();

    return $this->respondWithToken($user->createToken('MyApp')->plainTextToken);
}
```

</td>
</tr>
</table>

<br/>

<h4 style="font-size: 1.4rem;">iii. Profile Details</h4>

```sh
GET https://dev-api.gettonote.com/api/v1/user/profile
```

<table>
<tr>
<th>API Call</th>
</tr>
<tr>
<td>
<div style="width: 300px; margin-bottom: 30px">
<img src="./screenshots/profile.png" alt="Logo" width="300">
<p style="margin-bottom: 0px"><strong>Name: </strong>John Doe</p>
<p><strong>Email: </strong>user@tonote.com</p>
</div>
<div style="width: 300px; margin-bottom: 30px">
<img src="./screenshots/profile2.png" alt="Logo" width="300"> 
<p style="margin-bottom: 0px"><strong>Name: </strong>Woman Woman</p>
<p><strong>Email: </strong>wonderwoman@yahoo.com</p>
</div> 
<div style="width: 300px; margin-bottom: 30px">
<img src="./screenshots/profile3.png" alt="Logo" width="300"> 
<p style="margin-bottom: 0px"><strong>Name: </strong>Bill Gates</p>
<p><strong>Email: </strong>bill@tonote.com</p>
</div>
<p>./src/pages/Authentication/SignIn.tsx</p>
<p><strong>Register Screen</strong></p>  
</td>
<td>

```php
        return $this->showOne((new UserService())->userPropertyById(auth('api')->user()->id), 201);
```

</td>
</tr>
</table>

<br/>

### 2. Minting Players NFT 

```sh
POST https://dev-api.gettonote.com/api/v1/players
```

<table>
<tr>
<th>Component</th>
<th>API Call</th>
</tr>
<tr>
<td>
<div style="width: 400px; margin-right: 50px; margin-bottom: 20px">
<img src="./screenshots/file-upload-onChange.png" alt="Logo" width="600">
<p>Before upload</p> 
</div>
<div style="width: 400px; margin-right: 50px; margin-bottom: 20px">
<img src="./screenshots/file-upload-sent.png" alt="Logo" width="600">
<p>After upload</p>
</div>
<div style="width: 400px; margin-right: 50px; margin-bottom: 20px">
<img src="./screenshots/file-upload-too-large.png" alt="Logo" width="600">
<p>File too large</p>
</div>
<p>./src/pages/Dashboard/UploadDocument.tsx</p>
<p><strong>Upload Document Screen</strong></p>
</td>
<td>

```js
public function store(StorePlayerAttributeFormRequest $request)
{
    $existingUser = User::where('email', $request['email'])->orWhere('nin', $request['nin'])->first();

    $user = $existingUser ? $existingUser : User::create($request->except('attributes'));

    return (new ProcessNftService())->playerProcess($user, $request);

}
```

</td>
</tr>
</table>

<br/>

### 3. Displaying Document

<h4 style="font-size: 1.4rem;">i. Documents List</h4>

```sh
POST https://dev-api.gettonote.com/api/v1/documents
```

<table>
<tr>
<th>Component</th>
<th>API Call</th>
</tr>
<tr>
<td>

<img src="./screenshots/documents-list.png" alt="Logo" width="600">
<p>./src/pages/Dashboard/DocumentsList.tsx</p>
<p><strong>Documents List Screen</strong></p>

</td>
<td>

```js
// ./src/utils/types.ts
export type Token = {
  token?: string,
  token_type?: string,
};

// ./src/utils/api.ts
export function getDocuments(token: Token) {
  return axios({
    method: "get",
    url: `${API_URL}documents`,
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json;charset=UTF-8",
      Authorization: `Bearer ${token.token}`,
    },
  });
}
```

</td>
</tr>
</table>

<br/>

<h4 style="font-size: 1.4rem;">ii. View Document</h4>

```sh
GET https://dev-api.gettonote.com/api/v1/documents/{document_id}
```

<table>
<tr>
<th>Component</th>
<th>API Call</th>
</tr>
<tr>
<td>

<img src="./screenshots/view-document.png" alt="Logo" width="600">
<p>./src/pages/Dashboard/DocumentsList.tsx</p>
<p><strong>Documents List Screen</strong></p>

</td>
<td>

```js
// ./src/utils/types.ts
export type Token = {
  token?: string,
  token_type?: string,
};

// ./src/utils/api.ts
export function getDocument(document_id: string, token: Token) {
  return axios({
    method: "get",
    url: `${API_URL}documents/${document_id}`,
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json;charset=UTF-8",
      Authorization: `Bearer ${token.token}`,
    },
  });
}
```

</td>
</tr>
</table>

<br/>

### 5. Document Participants

i. Add Self as Participant

```sh
GET https://dev-api.gettonote.com/api/v1/document-participant-add-self/{document_id}
POST https://dev-api.gettonote.com/api/v1/document-participants
POST https://dev-api.gettonote.com/api/v1/document-participants-send-email
```

<table>
<tr>
<th>Component</th>
<th>API Call</th>
</tr>
<tr>
<td> 
<div style="width: 400px; margin-right: 50px; margin-bottom: 20px">
<img src="./screenshots/participants.png" alt="Logo" width="400"> 
<p>Retrieve Participants</p>
</div> 
<div style="width: 400px; margin-right: 50px; margin-bottom: 20px">
<img src="./screenshots/send-email-invite.png" alt="Logo" width="400"> 
<p>Display and Fill Form to Send Email Invite</p>
</div> 
<div style="width: 400px; margin-right: 50px; margin-bottom: 20px">
<img src="./screenshots/send-email-invite-2.png" alt="Logo" width="400"> 
<p>Email Invite Successfully Sent</p>
</div> 
<div style="width: 400px; margin-right: 50px; margin-bottom: 20px">
<img src="./screenshots/participants-2.png" alt="Logo" width="400"> 
<p>Participants List Updated (after page refresh)</p>
</div> 
<p>./src/pages/Dashboard/ViewDocument.tsx</p>
<p><strong>ViewDocument Screen</strong></p> 
</td>
<td>

```js
// ./src/utils/types.ts
export type Participant = {
  document_id: string,
  first_name: string,
  last_name: string,
  phone: string,
  email: string,
  role: string,
};

export type EmailInviteData = {
  message: string,
  files: string[] | ArrayBuffer[],
  participants: Participant[],
};

// ./src/utils/api.ts
export function addSelfAsParticipant(document_id: string, token: Token) {
  return axios({
    method: "get",
    url: `${API_URL}document-participant-add-self/${document_id}`,
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json;charset=UTF-8",
      Authorization: `Bearer ${token.token}`,
    },
  });
}

// This endpoint at
// POST https://dev-api.gettonote.com/api/v1/document-participants
// seems to not be working
// Participants data were retrieved from the `document` object
// `documentUploads` array where available.

export function sendParticipantEmailInvitation(
  data: EmailInviteData,
  token: Token
) {
  return axios({
    method: "post",
    url: `${API_URL}document-participants-send-email`,
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json;charset=UTF-8",
      Authorization: `Bearer ${token.token}`,
    },
    data,
  });
}
```

</td>
</tr>
</table>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[contributors-shield]: https://img.shields.io/github/contributors/Chizaram-Igolo/to-note.svg?style=for-the-badge
[contributors-url]: https://github.com/Chizaram-Igolo/to-note/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/Chizaram-Igolo/to-note.svg?style=for-the-badge
[forks-url]: https://github.com/Chizaram-Igolo/to-note/network/members
[stars-shield]: https://img.shields.io/github/stars/Chizaram-Igolo/to-note.svg?style=for-the-badge
[stars-url]: https://github.com/Chizaram-Igolo/to-note/stargazers
[issues-shield]: https://img.shields.io/github/issues/Chizaram-Igolo/to-note.svg?style=for-the-badge
[issues-url]: https://github.com/Chizaram-Igolo/to-note/issues
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/emmanueligolo
[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[React-url]: https://reactjs.org/
[Vite]: https://img.shields.io/badge/vite-%23646CFF.svg?style=for-the-badge&logo=vite&logoColor=white
[Vite-url]: https://vitejs.dev/
[TypeScript]: https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white
[TypeScript-url]: https://www.typescriptlang.org/
[TailwindCSS]: https://img.shields.io/badge/tailwindcss-%2338BDF8.svg?style=for-the-badge&logo=tailwind-css&logoColor=white
[TailwindCSS-url]: https://t
