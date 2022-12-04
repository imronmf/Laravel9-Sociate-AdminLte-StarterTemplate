=====SOCIATE=====

INSTALL JETSTREAM LIVEWIRE
composer require laravel/jetstream
php artisan jetstream:install livewire
npm install
npm ci && npm run dev
php artisan migrate

INSTALL LARAVEL SOCIALITE
composer require laravel/socialite

config/app.php
Laravel\Socialite\SocialiteServiceProvider::class,

Di aliases
'Socialite' => Laravel\Socialite\Facades\Socialite::class,

Config/services.php
'google' => [
        'client_id' => '………',
        'client_secret' => '………',
        'redirect' => '………',
    ],

CREATE GOOGLE APP

https://console.developers.google.com/apis/credentials?pli
-	Klik menu credentials
-	Pilih create credentials  OAuth client ID
-	Isi form, pastikan URL yang di inputkan sesuai
-	Simpan
-	Copy Client ID dan Client Secret ke services.php


SETTING AKHIR
-	Tambah kolom google_id tabel users
-	Edit model User
protected $fillable = [
        'name', 'email', 'password', 'google_id',
    ];
-	Buat controller : LoginWithGoogleController




<?php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Laravel\Socialite\Facades\Socialite;
use App\Models\User;
use Illuminate\Support\Facades\Auth;
use Exception;

class LoginWithGoogleController extends Controller
{
     public function redirectToGoogle()
    {
        return Socialite::driver('google')->redirect();
    }

    public function handleGoogleCallback()
    {
        try {
      
            $user = Socialite::driver('google')->user();
       
            $finduser = User::where('google_id', $user->id)->first();
       
            if($finduser){
       
                Auth::login($finduser);
      
                return redirect()->intended('dashboard');
       
            }else{
                $newUser = User::create([
                    'name' => $user->name,
                    'email' => $user->email,
                    'google_id'=> $user->id,
                    'password' => encrypt('123456dummy')
                ]);
      
                Auth::login($newUser);
                return redirect()->intended('dashboard');
            }
      
        } catch (Exception $e) {
            dd($e->getMessage());
        }
    }
}
-	Tambahkan route:
use App\Http\Controllers\LoginWithGoogleController;

Route::get('auth/google', [LoginWithGoogleController::class, 'redirectToGoogle']);
Route::get('auth/google/callback', [LoginWithGoogleController::class, 'handleGoogleCallback']);


-Tambah BUTTON Login

=====adminLTE=====

composer require jeroennoten/laravel-adminlte
php artisan adminlte:install
npm remove bootstrap
npm i bootstrap@4.6.1 -P
npm remove @popperjs/core

-Package lain
npm i @fortawesome/fontawesome-free
npm i icheck-bootstrap
npm i overlayscrollbars
npm i cross-env@7.0.3 -D
npm i jquery@3.6.0 -P
npm i popper.js@1.16.1 -P
npm i autoprefixer@10.4.5 -P
npm i resolve-url-loader@5.0.0 -D
npm i sass-loader@12.1.0 -D
npm i @lgaitan/pace-progress@1.0.7 -P
npm i @sweetalert2/theme-bootstrap-4@5.0.11 -P
npm i @ttskch/select2-bootstrap4-theme@1.5.2 -P
npm i bootstrap4-toggle@3.6.1 -P
npm i datatables.net-bs4@1.12.1 -P 
npm i datatables.net-buttons-bs4@2.2.3 -P
npm i frappe-charts@1.6.2 -P
npm i select2@4.0.13 -P
npm i sweetalert2@11.4.24 toastr@2.1.4 -P


ganti
"scripts": {
    "dev": "vite",
    "build": "vite build"
},

jadi
"scripts": {
    "dev": "npm run development",
    "development": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch": "npm run development -- --watch",
    "watch-poll": "npm run watch -- --watch-poll",
    "hot": "cross-env NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --disable-host-check --config=node_modules/laravel-mix/setup/webpack.config.js",
    "prod": "npm run production",
    "production": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --no-progress --config=node_modules/laravel-mix/setup/webpack.config.js"
},




resource/views/dashboard.blade.php

@extends('adminlte::page')

@section('title', 'Dashboard')

@section('content_header')
    <h1>Dashboard</h1>
@stop

@section('content')
    <p>Welcome to this beautiful admin panel.</p>
@stop

@section('css')
    <link rel="stylesheet" href="/css/admin_custom.css">
@stop

@section('js')
    <script> console.log('Hi!'); </script>
@stop




