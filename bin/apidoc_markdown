#!/usr/bin/env php
<?php

/*
 * Author   :    AlicFeng
 * Email    :    a@samego.com
 * Github   :    https://github.com/alicfeng
 * What php team of ev is that is 'one thing, a team, work together'
 */

// 1.CLI 参数处理
$opts = getopt('o:i:');
if (2 != count($opts)) {
    echo "Usage: apidoc_markdown [options...]\n" .
         "-i    input directory  \n" .
         "-o    output directory \n";
    exit(0);
}

if (false === file_exists($opts['i'])) {
    echo 'input dir not exists';
    exit(1);
}
file_exists($opts['o']) ? null : mkdir($opts['o'], 0777, true);

// 2.加载接口文档数据源数据
$api_data = json_decode(file_get_contents($opts['i'] . DIRECTORY_SEPARATOR . 'api_data.json'), true);

// 3.定义 readme 文件
$readme_path = $opts['o'] . DIRECTORY_SEPARATOR . 'readme.md';
file_exists($readme_path) ? unlink($readme_path) : null;
$readme_file    = fopen($readme_path, 'w');
$readme_content = '';

// 4.数据源数据分组
list($group_api_data, $group_name) = [[], 'group'];
foreach ($api_data as $value) {
    $group_api_data[$value[$group_name]][] = $value;
}
unset($api_data);

// 5.遍历分组生成对应的文件
foreach ($group_api_data as $group_name => $api_data) {
    // 5.1假设有生成过了则删除
    $api_doc_markdown_path = $opts['o'] . DIRECTORY_SEPARATOR . $group_name . '.md';
    if (file_exists($api_doc_markdown_path)) {
        unlink($api_doc_markdown_path);
    }

    // 5.2生成 md 文件
    $api_doc_markdown_file = fopen($api_doc_markdown_path, 'w');

    // 5.3处理 readme 标题
    $readme_content .= "\n#### " . $group_name . "\n";

    // 5.3编写目录内容 & 处理 readme 子标题
    foreach ($api_data as $item_data) {
        fwrite($api_doc_markdown_file, '- [x] [' . $item_data['title'] . '](#' . $item_data['name'] . ")\n");
        $readme_content .= '- [x] ' . $item_data['title'] . "\n";
    }

    // 5.4编写主体内容
    foreach ($api_data as $item_data) {
        // dom.title
        fwrite($api_doc_markdown_file, '#### ' . $item_data['title'] . "\n");

        // dom.description
        fwrite($api_doc_markdown_file, '> ' . $item_data['description'] . "\n");

        // dom.method & dom.url
        fwrite($api_doc_markdown_file, "```\n" . $item_data['type'] . '  ' . $item_data['url'] . "\n```\n");

        // dom.request_parameters
        if (array_key_exists('parameter', $item_data)) {
            fwrite($api_doc_markdown_file, "###### 请求参数\n");
            fwrite($api_doc_markdown_file, "| 字段 | 类型 | 必选 | 描述 | \n|:----:|:----:|:----:|:----:|\n");
            foreach ($item_data['parameter']['fields']['Parameter'] as $field) {
                fwrite($api_doc_markdown_file, '| `' . $field['field'] . '` | `' . $field['type'] . '` | ' . ($field['optional'] ? '否' : '是') . ' | ' . $field['description'] . " | \n");
            }
        }

        if (array_key_exists('success', $item_data)) {
            // dom.response_parameters
            fwrite($api_doc_markdown_file, "###### 响应参数\n");
            if (array_key_exists('fields', $item_data['success'])) {
                foreach ($item_data['success']['fields'] as $key => $response_packages) {
                    fwrite($api_doc_markdown_file, "- $key\n");
                    fwrite($api_doc_markdown_file, "| 字段 | 类型 | 必选 | 描述 | \n|:----:|:----:|:----:|:----:|\n");
                    foreach ($response_packages as $field) {
                        fwrite($api_doc_markdown_file, '| `' . $field['field'] . '` | `' . $field['type'] . '` | ' . ($field['optional'] ? '否' : '是') . ' | ' . $field['description'] . " | \n");
                    }
                }
            }

            //  dom.success_simple
            fwrite($api_doc_markdown_file, "###### 成功响应示例\n");
            foreach ($item_data['success']['examples'] as $example) {
                fwrite($api_doc_markdown_file, '- ' . $example['title'] . "\n");
                fwrite($api_doc_markdown_file, "```json\n" . json_encode(json_decode($example['content']), JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE) . "\n```\n");
            }
        }

        if (array_key_exists('error', $item_data)) {
            //  dom.success_simple
            fwrite($api_doc_markdown_file, "###### 失败响应示例\n");
            foreach ($item_data['error']['examples'] as $example) {
                fwrite($api_doc_markdown_file, '- ' . $example['title'] . "\n");
                fwrite($api_doc_markdown_file, "```json\n" . json_encode(json_decode($example['content']), JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE) . "\n```\n");
            }
        }

        fwrite($api_doc_markdown_file, "\n\n");
    }

    // 5.5关闭文件
    fclose($api_doc_markdown_file);
}

// 6. readme
fwrite($readme_file, $readme_content) && fclose($readme_file);
