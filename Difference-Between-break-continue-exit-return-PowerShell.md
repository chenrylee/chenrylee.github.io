It's a PowerShell script:
```powershell
'Starting'
function Test-Function {
    $fishtank = 1..10
    Foreach ($fish in $fishtank)
    {
        if ($fish -eq 7)
        {
            break      # <- abort loop
            #continue  # <- skip just this iteration, but continue loop
            #return    # <- abort code, and continue in caller scope
            #exit      # <- abort code at caller scope 
        }
        "fishing fish #$fish"
    }
    'Done.'
}
Test-Function

'Script done!'
```

If you change the action, you will get different output results:
- Break:

> Starting  
fishing fish #1  
fishing fish #2  
fishing fish #3  
fishing fish #4  
fishing fish #5  
fishing fish #6  
Done.  
Script done!  

- Continue:
> Starting  
fishing fish #1  
fishing fish #2  
fishing fish #3  
fishing fish #4  
fishing fish #5  
fishing fish #6  
fishing fish #8  
fishing fish #9  
fishing fish #10  
Done.  
Script done!  

- Return:  
> Starting  
fishing fish #1  
fishing fish #2  
fishing fish #3  
fishing fish #4  
fishing fish #5  
fishing fish #6  
Script done!  
	

- Exit:
> Starting  
fishing fish #1  
fishing fish #2  
fishing fish #3  
fishing fish #4  
fishing fish #5  
fishing fish #6  
