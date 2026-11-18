| page_title | unifi_site Resource - terraform-provider-unifi | 
|---|---|
| subcategory |  | 
| description | unifi_site manages Unifi sites | 
unifi_site manages Unifi sites
resource "unifi_site" "mysite" {
  description = "mysite"
}
- description (String) The description of the site.
- id (String) The ID of the site.
- name (String) The name of the site.
Import is supported using the following syntax:
# import using the API/UI ID
terraform import unifi_site.mysite 5fe6261995fe130013456a36
# import using the name (short ID)
terraform import unifi_site.mysite vq98kwez