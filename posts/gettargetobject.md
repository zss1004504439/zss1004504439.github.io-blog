---
id: JsvNfK
title: getTargetObject
createdAt: "2019-09-19 18:16:00"
updated: "2026-04-17 11:19:42"
tags:
    - js utils
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

/**
 *
 * @param {Object} targetObject
 * @param {Array} propsArray
 */
export function getTargetObject(targetObject, propsArray) {
  if (typeof (targetObject) !== 'object' || !Array.isArray(propsArray)) {
    throw new Error('参数格式不正确')
  }
  const result = {}
  Object.keys(targetObject).filter(key => propsArray.includes(key)).forEach(key => {
    result[key] = targetObject[key]
  })
  return result
}