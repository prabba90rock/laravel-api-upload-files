<?php

namespace App\Services;

// use GuzzleHttp\Client;
use Illuminate\Support\Facades\Http;
use Illuminate\Support\Facades\File;

class UploadApi
{
    protected $url;
    protected $headers;

    public function __construct()
    {
        $this->url = env('YOUR_API_URL');

    }

    public function getReadingsAfterUpload()
    {

        $destination = storage_path('folder/path/to/your/file');

        // 1st option - using laravel Facades\Http
        try {
            
            $filePath = $destination . '/your-file-name'; // add with the extension
     
            $response = Http::attach('file', fopen($filePath, 'r'))->post($url);

            $response = Http::attach(
                'file_name', fopen($filePath, 'r'), 'filename.jpg'
            )->post($this->url, [
                'key1' => 'value1', // addtional params
                'key2' => 'value2',
            ]);


            dd($response->json());

        } catch (RequestException $e) {
            dd($e->response->status(), $e->response->body());
        }

        // using curl

    
        $filePath = $destination . '/your-file-name';

        $ch = curl_init();

        curl_setopt($ch, CURLOPT_URL, $this->url);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_POST, true);
        curl_setopt($ch, CURLOPT_HTTPHEADER, array(
            'Content-Type: multipart/form-data',
            'Accept: image/png',
            'Authorization: Bearer '.$KEY
        ));

        $postFields = array(
            'file' => new \CurlFile($file)
        );

        curl_setopt($ch, CURLOPT_POSTFIELDS, $postFields);

        $result = curl_exec($ch);

        dd(json_decode($result));

        curl_close($ch);


    }

}
