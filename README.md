      - uses: kkurno/action-markdown-reader@v0.1.0
        id: markdown
        with:
          markdown: |
            # Main Topic - My Markdown

            This is an example
            
            ## Sub Topic
            
            - [x] list item 1
            - [ ] list item 2

      - name: See the full result
        run: |-
          echo ${{ toJSON(steps.markdown.outputs.data) }}

      - name: See specific properties
        run: |-
          echo 'should be "Main Topic - My Markdown" --> ${{ fromJSON(steps.markdown.outputs.data).Main_Topic___My_Markdown.text }}'
          echo 'should be "This is an example" --> ${{ fromJSON(steps.markdown.outputs.data).Main_Topic___My_Markdown.bodies[0].text }}'
          echo 'should be "true" --> ${{ fromJSON(steps.markdown.outputs.data).Main_Topic___My_Markdown.subheader.Sub_Topic.bodies[0].items[0].checked }}'
          echo 'should be "false" --> ${{ fromJSON(steps.markdown.outputs.data).Main_Topic___My_Markdown.subheader.Sub_Topic.bodies[0].items[1].checked }}'

      - name: Should be skip
        if: fromJSON(steps.markdown.outputs.data).Main_Topic___My_Markdown.subheader.Sub_Topic.bodies[0].items[1].checked
        run: |-
          echo "2nd item is checked"
