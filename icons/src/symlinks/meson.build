symlinks_kinds = {
  'fullcolor': OPTIONAL_ICONS_SIZES + ICONS_SIZES,
  'symbolic': ['scalable'],
}

# We can't use a dict as public entry point, because dicts are not ordered
ICONS_SYMLINKS = []

foreach kind, sizes: symlinks_kinds
  foreach category : ICONS_CATEGORIES
    list_file = kind / category + '.list'
    links_map = {}
    message('Parsing symlinks list', list_file)
    if fs.exists(list_file)
      foreach line : fs.read(list_file).strip().split('\n')
        line = line.strip()
        foreach i: range(1000)
          if line.contains('  ')
            line = line.replace('  ', ' ')
          else
            break
          endif
        endforeach

        if line == ''
          continue
        endif

        line_split = line.split(' ')
        assert(line_split.length() == 2, 'Unexpected line: ' + line)
        target = line_split[0]
        link = line_split[1]

        if link in links_map
          warning('Duplicated link definition for @0@, was: @1@'.format(
              link, links_map[link]))
        else
          links_map += { link: target }
        endif

        foreach size : sizes
          sized_target = target
          sized_link = size / category / link
          ICONS_SYMLINKS += [[sized_link, sized_target]]
          # message('@0@ => @1@'.format(sized_link, sized_target))
        endforeach
      endforeach
    endif
  endforeach
endforeach


      # foreach link, target: LINKS
      #   link_path = fs.parent(link)
      #   category = fs.name(link_path)
      #   size = fs.parent(link_path)
      #   src_file = subdir / link_path / target

      #   if fs.is_file(src_file)
      #     install_symlink(fs.name(link),
      #       pointing_to: target,
      #       install_dir: theme_install_dir / fs.parent(link),
      #     )
      #   elif not is_flavour and not optional_icon_sizes.contains(size)
      #     error('@0@ is not a file in default variant!'.format(src_file))
      #   endif
      # endforeach
