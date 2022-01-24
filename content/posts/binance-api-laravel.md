---
title: "Binance API with Laravel"
date: 2022-01-16T22:20:16+01:00
draft: false
categories : [ "laravel" ]
tags : ["binance", "laravel"]
---
## Fetch all currencies (Artisan command)
```PHP
<?php

namespace App\Console\Commands;

use App\Models\Currency;
use Illuminate\Console\Command;
use Illuminate\Support\Facades\Http;

class BinanceImportCurrencies extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'binance:currencies';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Import Binance currencies';

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return int
     */
    public function handle()
    {
        $timestamp = time() * 1000;
        $signature = hash_hmac('SHA256', 'timestamp=' . $timestamp, env('BINANCE_SECRET_KEY'));

        $response = Http::withHeaders([
            'X-MBX-APIKEY' => env('BINANCE_API_KEY')
        ])->get('https://api.binance.com/sapi/v1/capital/config/getall', [
            'timestamp' => $timestamp,
            'signature' => $signature
        ]);
    }
}
```

---

[Binance API Documenation](https://binance-docs.github.io/apidocs/spot/en/#change-log)