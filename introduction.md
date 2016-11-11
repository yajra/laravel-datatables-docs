# Introduction

## jQuery DataTables API for Laravel

[![Join the chat at https://gitter.im/yajra/laravel-datatables](https://badges.gitter.im/yajra/laravel-datatables.svg)](https://gitter.im/yajra/laravel-datatables?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![License](https://poser.pugx.org/yajra/laravel-datatables-oracle/license)](https://packagist.org/packages/yajra/laravel-datatables-oracle)

[![Laravel 4.2|5.x](https://img.shields.io/badge/Laravel-4.2|5.x-orange.svg)](http://laravel.com)
[![Latest Stable Version](https://poser.pugx.org/yajra/laravel-datatables-oracle/v/stable)](https://packagist.org/packages/yajra/laravel-datatables-oracle)
[![Build Status](https://travis-ci.org/yajra/laravel-datatables.svg?branch=master)](https://travis-ci.org/yajra/laravel-datatables)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/yajra/laravel-datatables/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/yajra/laravel-datatables/?branch=master)
[![Total Downloads](https://poser.pugx.org/yajra/laravel-datatables-oracle/downloads)](https://packagist.org/packages/yajra/laravel-datatables-oracle)

This package is created to handle [server-side](https://www.datatables.net/manual/server-side) works of [DataTables](http://datatables.net) jQuery Plugin via [AJAX option](https://datatables.net/reference/option/ajax) by using Eloquent ORM, Fluent Query Builder or Collection.

```php
use Yajra\Datatables\Facades\Datatables;

// Using Eloquent
return Datatables::eloquent(User::query())->make(true);

// Using Query Builder
return Datatables::queryBuilder(DB::table('users'))->make(true);

// Using Collection
return Datatables::collection(User::all())->make(true);

// Using the Engine Factory
return Datatables::of(User::query())->make(true);
return Datatables::of(DB::table('users'))->make(true);
return Datatables::of(User::all())->make(true);
```
