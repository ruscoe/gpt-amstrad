# GPT Amstrad

Training a ChatGPT model to act like an Amstrad PCW8512.

## Steps

### 1. Use ChatGPT to generate a fine-tuning file

Prompt: "Generate a fine tuning json file that makes an AI model act like an Amstrad PCW 8256."

Response: amstrad_pcw8256_finetune.jsonl

### 2. Use the OpenAI API to fine-tune a model

For this I'm using an [OpenAI PHP library](https://github.com/ruscoe/openai-php) that I wrote. Specifically the [fine-tuning functionality](https://github.com/ruscoe/openai-php?tab=readme-ov-file#fine-tuning-a-model).

```php
<?php

require __DIR__ . '/vendor/autoload.php';

// @see https://platform.openai.com/docs/api-reference/authentication
$api_key = getenv('OPENAI_API_KEY');

$api = new OpenAI\OpenAIFiles($api_key);

$file = $api->uploadFile('amstrad_pcw8256_finetune.jsonl');
```

### 3. Get the new file ID

```php
<?php

require __DIR__ . '/vendor/autoload.php';

// @see https://platform.openai.com/docs/api-reference/authentication
$api_key = getenv('OPENAI_API_KEY');

$api = new OpenAI\OpenAIFiles($api_key);

$files = $api->getFiles();

var_dump($files);
```

Resulting file: `FILE_NAME`

### 4. Create a new training job

```php
<?php

require __DIR__ . '/vendor/autoload.php';

// @see https://platform.openai.com/docs/api-reference/authentication
$api_key = getenv('OPENAI_API_KEY');

$api = new OpenAI\OpenAIFineTunes($api_key);

$api->create('gpt-4o-mini-2024-07-18', 'FILE_NAME');
```

Resulting fine-tuned model: `MODEL_NAME`

### 5. Start interacting!

```php
<?php

require __DIR__ . '/vendor/autoload.php';

// @see https://platform.openai.com/docs/api-reference/authentication
$api_key = getenv('OPENAI_API_KEY');

$api = new OpenAI\OpenAICompletions($api_key);

$messages = [
    (object) ['role' => 'user', 'content' => 'What is your name?'],
];

$response = $api->create('FINE_TUNED_MODEL_NAME', $messages);
```

Reponse: `RESPONSE`
