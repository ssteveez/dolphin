# dolphin
A vulnerability classified as serious was found in DolphinPHP V1.5.1. Operation on parameter ids causes remote command execution


from http://www.dolphinphp.com/getDolphin.html Download the latest version of DolphinPHP V1.5.1 source code From the code audit,We can see that the code in /application/common.php has calls whose parameters can be controlled_ user_ Func method
![图片](https://user-images.githubusercontent.com/125523284/219248886-94cbaa8e-99f5-4729-baa9-3dd8d0b05a4d.png)
Where, the controllable parameters are param [1] and log [$param [0]]
First, param is the value separated by |
And value is actually the traversal of match [1]
Match is through regular matching, action_ Info ['log']. This rule is the matching value in brackets, and the final $action_ Info is obtained from database query
![图片](https://user-images.githubusercontent.com/125523284/219249667-b5701783-20a3-41ed-b9b9-f8afa4cf0019.png)
![图片](https://user-images.githubusercontent.com/125523284/219249780-19091fd0-6cfa-4ae0-8946-7ee6a3b3d3ca.png)
But we noticed that we need to bypass the judgment of is_disable_func($param[1])
![图片](https://user-images.githubusercontent.com/125523284/219249969-cdcc934a-93c9-4c87-86cc-c0b1825f56dd.png)

Then we find the list of disable function
![图片](https://user-images.githubusercontent.com/125523284/219250133-58105c94-8bb3-4e66-80d9-f1038e5dc98e.png)
Then through the shell_exec() method attempts to execute the command
Set the log rules of the "Delete Attachment" function in the "Behavior Management" option
![图片](https://user-images.githubusercontent.com/125523284/219251224-90042a0a-3405-4f42-b3a2-442e2fbe6c91.png)
Modify the rule to [details|shell_exec] test ([details])
![图片](https://user-images.githubusercontent.com/125523284/219251679-3dbdda5a-f1c9-4530-b06a-2b746e0017d5.png)
When deleting an attachment, execute the command through ids[]=calc%26&ids[]=x（X is the attachment id）
![图片](https://user-images.githubusercontent.com/125523284/219252248-47470c99-0252-490f-8150-d24ebf118dd1.png)
![图片](https://user-images.githubusercontent.com/125523284/219254771-52be102c-b06d-47a2-aaf9-6f96a796f5de.png)
![图片](https://user-images.githubusercontent.com/125523284/219255213-494de84a-ca92-4b26-9ed3-8285d8942c62.png)
