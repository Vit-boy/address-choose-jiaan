# address-choose-jiaan
选择地址三级联动，省市区

#调用方法

npm i address-choose-jiaan

import {AddressChooseJiaan} from 'address-choose-jiaan'

<address-choose-jiaan 
  :provinceId="省份ID" 
  :countryId="市区ID" 
  :cityId="区域ID"
  @handleCancel="取消按钮触发方法" 
  @handleSubmit="确认按钮触发方法"
 >
 </address-choose-jiaan>
 
 #handleSubmit 回调中参数
 province-省份ID
 country-城市ID
 city-区域ID
 value-选择后的中文名称
        
